---
title: "Loose Review Instructions"
owners: [bingran-you, cryppadotta, serenakeyitan]
soft_links: [engineering/backend/heartbeat-run-orchestration/NODE.md, product/governance/issue-approvals/NODE.md, product/task-system/NODE.md, product/task-system/issue-thread-interactions/NODE.md]
---

# Loose Review Instructions

When an agent finishes work on an issue and hands off to a reviewer through the execution policy, it can attach free-form **review instructions** to the handoff. These instructions guide the reviewer (human or agent) about what to focus on — e.g. "check failure handling" or "verify against the reproduction steps" — without being structured into the policy itself.

## Key Decisions

### Loose Instructions, Not Structured Checklists

The `reviewRequest` payload carries a single `instructions` string (1–20,000 chars, trimmed). It is intentionally unstructured — the coder writes prose for the reviewer. The system does not parse, validate, or enforce specific review criteria; it just propagates the text. This keeps the handoff lightweight and avoids prematurely formalizing a review checklist schema.

### Lives on Execution State, Cleared on Transition

`reviewRequest` is stored on `IssueExecutionState` alongside `currentParticipant` and `returnAssignee`. When a stage transitions (review approved / changes requested), the field is cleared so stale instructions don't leak across stages. Callers can also explicitly null it during a transition.

### Surfaced in the Reviewer's Wake Prompt

When the reviewer is woken via heartbeat, the rendered Paperclip wake prompt includes the instructions verbatim under "Review request instructions:". This is how the reviewer agent actually sees the guidance — it is not a UI-only field.

### Set Through the Issue Update Endpoint

The `PATCH /issues/:id` endpoint accepts an optional `reviewRequest` field. An agent providing only `reviewRequest` (with no other update fields) still counts as an agent work update, so the handoff fires even when the coder is just attaching review guidance alongside a status change to `done` / `in_review`.
