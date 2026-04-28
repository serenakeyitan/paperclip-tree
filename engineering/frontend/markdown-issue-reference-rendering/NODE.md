---
title: "Markdown Issue Reference Rendering"
owners: [bingran-you, cryppadotta, serenakeyitan]
soft_links: [engineering/frontend/issue-editor-reliability/NODE.md, engineering/frontend/issue-thread-ux/NODE.md, product/task-system/issue-references/NODE.md]
---

# Markdown Issue Reference Rendering

How issue references (e.g. `PAP-1271`, `/PAP/issues/PAP-1271`, `issue://PAP-1310`) are detected in markdown content and rendered inline as lightweight links rather than full pill chips.

## Key Decisions

### Inline, Not Pill

Issue references inside markdown bodies render as plain inline links with a leading `StatusIcon` and the identifier as the label. The dedicated `paperclip-markdown-issue-ref` class keeps the icon+label pair on a single line (`white-space: nowrap`) without applying the rounded pill chrome used for `IssueReferencePill` in property rows. Pills are still used in structured contexts like the Blocked-by row in `IssueProperties`, but inline prose stays visually quiet.

### Accessible Labels Adapt to Available Title

`MarkdownIssueLink` looks up the issue via `issuesApi` and computes the `aria-label` from the resolved title: `Issue PAP-1271` when the title is unknown or equal to the identifier, and `Issue PAP-1271: Fix hover state` once a distinct title is loaded. This avoids redundant `Issue PAP-1271: PAP-1271` labels while still announcing useful context to screen readers when titles are available.

### Case-Insensitive Path Parsing

`parseIssuePathIdFromPath` and `parseIssueReferenceFromHref` accept lowercased identifiers in URL paths (e.g. `/PAP/issues/pap-1272`) and normalize them to upper-case canonical form before routing. The link's `to` is rebuilt from the resolved identifier rather than the original href, so casing variations all collapse to a single canonical issue URL.

### Tokenizer Recognizes Path-Style References And Brackets

`ISSUE_REFERENCE_TOKEN_RE` was extended to match path-style tokens like `/PAP/issues/PAP-1271` directly inside prose, in addition to bare identifiers, full URLs, and the `issue://` scheme. Trailing-punctuation splitting now also balances `]` (in addition to `)`), so references inside markdown link/footnote brackets are detected correctly without swallowing the closing bracket.

### Multiline InlineEditor Uses MarkdownBody Preview

When `InlineEditor` is in multiline mode with a non-empty value, it renders `MarkdownBody` as the preview surface. Clicking the preview enters edit mode; this lets descriptions show resolved issue references at rest while still giving users a single click into the editor.

## Source

- `ui/src/components/MarkdownBody.tsx` — `MarkdownIssueLink` rendering and aria-label rules
- `ui/src/lib/issue-reference.ts` — tokenizer, path parser, href normalization
- `ui/src/components/IssueReferencePill.tsx` — pill variant (accepts `children` override)
- `ui/src/components/InlineEditor.tsx` — multiline `MarkdownBody` preview integration
- `ui/src/index.css` — `.paperclip-markdown-issue-ref` styling
