---
title: "Stale Queued Run Invalidation"
owners: [bingran-you, cryppadotta, serenakeyitan]
soft_links: [engineering/backend/heartbeat-run-orchestration/NODE.md, product/task-system/issue-blockers/NODE.md, product/task-system/non-terminal-liveness-contract/NODE.md]
---

When a queued heartbeat run is claimed, the heartbeat service re-checks the underlying issue state before dispatch. If the issue graph has shifted since the run was queued, the run is cancelled instead of executed, preventing agents from acting on stale intent.

## Key Decisions

### Re-Validate at Claim Time

`claimQueuedRun` calls `evaluateQueuedRunStaleness` immediately before dispatch. Validation runs at claim time (not enqueue time) because the gap between queueing and execution is where issue graphs drift — assignees change, issues reach terminal status, review participants are re-typed.

### Staleness Reasons

A queued run is considered stale for one of four typed reasons, each surfaced as an `errorCode`: `issue_not_found`, `issue_assignee_changed`, `issue_terminal_status`, or `issue_review_participant_changed`. Stale runs are cancelled via `cancelQueuedRunForStaleIssue` with the reason recorded for audit.

### Coordinated With Blocker Cancellation

Staleness invalidation runs alongside the existing blocked-issue cancellation pass. Both share the same claim-time hook so a queued run is cancelled exactly once for the highest-priority reason (blocker, then staleness) before any adapter execution.
