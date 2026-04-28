---
title: "Multiline Text Normalization"
owners: [bingran-you, cryppadotta, serenakeyitan]
soft_links: [engineering/shared/NODE.md, product/governance/issue-approvals/NODE.md, product/task-system/NODE.md, product/task-system/issue-thread-ux/NODE.md]
---

Free-text fields that accept multiline content (issue descriptions, issue/approval comments, decision notes) run their input through a shared `multilineTextSchema` Zod transform in `packages/shared/src/validators/text.ts`. The transform calls `normalizeEscapedLineBreaks`, which rewrites literal `\r\n`, `\n`, and `\r` sequences into real `\n` newlines before downstream validation runs.

## Why This Exists

Agents and external clients frequently submit JSON payloads where line breaks were double-escaped — for example, a description like `"PR: ...\\n\\nShip the follow-up."` arrives as the literal string `PR: ...\n\nShip the follow-up.` instead of containing real newlines. Without normalization, that text renders as a single run-on paragraph in the UI. Centralizing the fix in one schema means every multiline field gets the same treatment, and real `\n` characters in well-formed input pass through unchanged.

## Where It Applies

- Issue: `createIssueSchema.description`, `updateIssueSchema.comment`, `addIssueCommentSchema.body`, `upsertIssueDocumentSchema` content, `suggestedTaskDraftSchema.description`, `respondIssueThreadInteractionSchema` response text.
- Approval: `resolveApprovalSchema.decisionNote`, `requestApprovalRevisionSchema.decisionNote`, `addApprovalCommentSchema.body`.

When `min(1)` or `trim().max(...)` constraints are required, compose with `.pipe(z.string().min(1))` rather than replacing the transform — the normalization must run first so trimming and length checks see the real newline content.

## Convention For New Multiline Fields

Any new validator that accepts user- or agent-authored prose should use `multilineTextSchema` instead of a bare `z.string()`. Single-line fields (titles, identifiers, labels) should keep `z.string()` so escaped-backslash sequences remain literal.
