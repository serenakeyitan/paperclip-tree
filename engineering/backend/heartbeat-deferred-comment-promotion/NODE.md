---
title: "Deferred Comment Wake Promotion"
owners: [bingran-you, cryppadotta, serenakeyitan]
soft_links: ["engineering/backend/heartbeat-run-orchestration/NODE.md", "product/task-system/comment-wake/NODE.md"]
---

# Deferred Comment Wake Promotion

How the backend handles comments that arrive while an issue already has an active heartbeat run. Instead of starting a second overlapping run immediately, the wake is deferred and then promoted into a follow-up run during the active run's release step.

**Source:** `server/src/services/heartbeat.ts`, `server/src/routes/issues.ts`, `server/src/services/issues.ts`

## Key Decisions

### Comments During Active Execution Defer Rather Than Overlap

When a new issue comment arrives while the assigned agent already has an active run for that issue, the backend records a deferred wake request instead of launching a second concurrent execution. This preserves the low-latency comment-wake model without allowing overlapping runs to race on the same issue.

### Promotion Happens During Release And Reuses Issue Context

The deferred wake is promoted as part of `releaseIssueExecutionAndPromote`, after the active run releases execution on the issue. When the run includes an `issueId` in its context snapshot, the release step locks that specific issue row with `FOR UPDATE` rather than locking by `execution_run_id` alone. This narrows lock scope and keeps unrelated issues from contending on the same release path.

### Deferred Comment Promotion Can Reopen Closed Issues

If the active run closes the issue and deferred comment wakes exist for that same issue, promotion reopens the issue to `todo` before queuing the follow-up run. The promoted context carries the reopen provenance, and the backend logs a system activity entry with source `deferred_comment_wake` so the reopen is visible in audit history.

### Automatic Run Summary Comments Are Fallback-Only

Heartbeat finalization checks for an existing issue comment already tagged with the same `runId` before posting its generated run summary. If the run itself already wrote a completion comment, the fallback summary is suppressed and the run is marked satisfied by that existing comment. This prevents duplicate run-summary comments when finalize or promotion paths retry.

## Boundaries

- The decision to wake an agent on new comments remains part of [Comment Wake](../../../product/task-system/comment-wake/NODE.md).
- This node covers the backend-side release, reopening, dedupe, and promotion mechanics after a comment wake collides with an already-running issue execution.
