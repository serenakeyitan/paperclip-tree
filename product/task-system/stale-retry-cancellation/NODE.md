---
title: "Stale Retry Cancellation on Ownership/Status Change"
owners: [bingran-you, cryppadotta, serenakeyitan]
soft_links: [engineering/backend/heartbeat-run-orchestration/NODE.md, product/task-system/NODE.md, product/task-system/run-liveness-continuations/NODE.md]
---

# Stale Retry Cancellation on Ownership/Status Change

When a heartbeat run is parked in `scheduled_retry` waiting for its next attempt, the issue it belongs to may change underneath it: a different agent may be assigned, or the issue may be cancelled outright. Letting the original retry fire in those cases would either run work for the wrong assignee or revive an issue the user already gave up on. The retry promotion path now detects these stale cases and cancels the retry instead of promoting it.

## Key Decisions

### Stale Detection at Promotion Time, Not at Reassignment Time

The check happens inside the heartbeat retry-promotion loop in `server/src/services/heartbeat.ts`: for each due `scheduled_retry` run whose `contextSnapshot` carries an `issueId`, the service re-reads the issue and compares `assigneeAgentId` and `status`. If the assignee changed or the issue is `cancelled`, the run is transitioned to `cancelled` with an explicit `errorCode` (`issue_reassigned` or `issue_cancelled`) rather than being promoted to `queued`. This avoids racing against every reassignment write path and keeps the cancellation logic in one place.

### New Assignees Are Not Deferred Behind Old Retries

The accompanying test asserts that when ownership flips, the new assignee is not blocked by the previous assignee's pending retry — the stale retry is cancelled, freeing the new assignee to schedule fresh work immediately.

### Issue Status `cancelled` Cancels the Active Run Synchronously

In parallel, the issue PATCH route (`server/src/routes/issues.ts`) now resolves the active run for an issue when its status transitions to `cancelled` and calls `heartbeat.cancelRun` inline, emitting a `heartbeat.cancelled` activity log entry with `source: issue_status_cancelled`. This complements the retry-promotion guard: in-flight runs are stopped at the moment of cancellation, while parked retries are stopped lazily when they come due.

### Distinct Error Codes for Diagnostics

`issue_reassigned` and `issue_cancelled` are emitted as `errorCode` on the cancelled retry so dashboards and audit trails can distinguish ownership churn from explicit user cancellation.
