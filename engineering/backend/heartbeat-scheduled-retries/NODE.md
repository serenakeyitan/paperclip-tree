---
title: "Heartbeat Scheduled Retries"
owners: [bingran-you, cryppadotta, serenakeyitan]
soft_links: [adapters/codex-local/NODE.md, engineering/backend/heartbeat-run-orchestration/NODE.md, engineering/database/NODE.md]
---

# Heartbeat Scheduled Retries

Heartbeat runs can be **scheduled to retry** after a transient failure rather than being marked terminally failed. Each `heartbeat_runs` row carries `scheduled_retry_at`, `scheduled_retry_attempt`, and `scheduled_retry_reason` columns that drive a deferred retry instead of immediate re-execution.

## Key Decisions

### Retry Is Deferred, Not Immediate

When a run hits a retryable condition (for example, a transient upstream error from the Codex adapter), the orchestrator stamps `scheduled_retry_at` with the next attempt time and increments `scheduled_retry_attempt`. The run is not re-dispatched in-line; the heartbeat scheduler picks it up at or after `scheduled_retry_at`. This keeps retry logic uniform with the rest of the heartbeat lifecycle and avoids tight loops on persistent upstream issues.

### Retry Reason Is Recorded

`scheduled_retry_reason` captures *why* the run was scheduled for retry (e.g. transient upstream / remote-compaction high-demand). This is observable in run history and used by tests to assert the right path was taken (`heartbeat-retry-scheduling.test.ts`, `heartbeat-process-recovery.test.ts`).

### Distinct From Process-Loss Recovery

`scheduled_retry_*` is separate from `process_loss_retry_count`. Process-loss recovery handles workers that died mid-run; scheduled retries handle runs that completed but produced a retryable failure shape. The two counters do not share state.

## Cross-Domain

The Codex local adapter classifies which failures are eligible (see `isCodexTransientUpstreamError` in the parse module). Adapter classification is the input; scheduling and dispatch are owned by the backend heartbeat service.
