---
title: "Implicit Reopen on Agent Comment"
owners: [bingran-you, cryppadotta, serenakeyitan]
soft_links: ["product/task-system/comment-wake/NODE.md", "product/task-system/issue-blockers/NODE.md", "engineering/frontend/issue-thread-ux/NODE.md"]
---

# Implicit Reopen on Agent Comment

Comments can move work back to `todo` without a separate reopen action when the comment targets an agent assignee. This applies to `done` and `cancelled` issues, and also to `blocked` issues that are resumable because they have no unresolved first-class blockers.

## Key Decisions

### Agent-Targeted Comments Act as Resume Signals

When a comment is posted through either the issue update path or the dedicated comment path, Paperclip treats it as a signal to resume work if the resulting assignee is an agent and the issue is in a resumable state. The backend is the source of truth: it performs the status transition back to `todo` and records `reopened` plus `reopenedFrom` in activity.

### UI Predicts the Outcome but Does Not Own It

The issue thread composer no longer exposes a manual reopen toggle. Instead, the UI predicts reopen based on the current issue status and the comment's assignment target so humans can reassign and comment in one step. The server independently enforces the same workflow semantics so direct API callers and automation follow the same rule.

### Self-Authored Agent Comments Do Not Implicitly Reopen

Implicit reopen is suppressed when the commenting actor is the same agent currently assigned to the issue. This prevents an agent from accidentally bouncing its own completed work back to `todo` by leaving a closing or follow-up comment.

### Blocker Semantics Still Win

A `blocked` issue only resumes through this path when it is manually blocked and has no unresolved first-class blockers. Dependency-blocked work stays `blocked` until the blocker relation is actually cleared; comments alone do not override blocker semantics.

## Boundaries

- Comment-triggered wake delivery after the reopen is defined in [Comment Wake](../comment-wake/NODE.md).
- Dependency semantics for `blocked` work are defined in [Issue Blockers](../issue-blockers/NODE.md).
- The composer interaction and hidden reopen control are frontend concerns described in [engineering/frontend/issue-thread-ux](../../../engineering/frontend/issue-thread-ux/NODE.md).
