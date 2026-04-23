---
title: "Heartbeat Retry Scheduling"
owners: [bingran-you, cryppadotta, serenakeyitan]
soft_links: ["engineering/backend/heartbeat-run-orchestration/NODE.md", "adapters/codex-local/NODE.md", "engineering/database"]
---

# Heartbeat Retry Scheduling

Heartbeat runs that fail due to transient upstream conditions are not surfaced as terminal failures — they are re-scheduled with backoff and tracked on the run row itself. The `heartbeat_runs` table carries three columns that together form the retry contract: `scheduled_retry_at` (when the next attempt should fire), `scheduled_retry_attempt` (monotonic attempt counter, default 0), and `scheduled_retry_reason` (free-form classification of why the retry was scheduled).

## Key Decisions

### Separate scheduled retries from process-loss retries

The existing `process_loss_retry_count` column covers cases where the worker process died. Scheduled retries are a distinct concern — the process finished cleanly but the upstream call failed in a way that is expected to recover. Keeping the two counters separate lets orchestration apply different caps and backoff policies without conflating crash recovery with upstream flakiness.

### Transient upstream classification is narrow by design

The Codex local adapter exposes `isCodexTransientUpstreamError`, which only fires when both a transient marker ("high demand", "temporary errors", 429, "service unavailable", etc.) **and** a remote-compaction/high-demand context are present. Broader 429s are deliberately not treated as transient because they are more likely to be account- or user-scoped rate limits that should not be retried silently. Deterministic compaction errors (invalid_request_error, unknown parameters) are explicitly excluded.

### Codex transient fallback modes

When a transient upstream error is observed, the adapter can choose one of four fallback modes via the `codexTransientFallbackMode` context flag: `same_session`, `safer_invocation`, `fresh_session`, or `fresh_session_safer_invocation`. "Safer invocation" variants drop optional features (e.g. remote compaction) that triggered the failure; "fresh session" variants abandon the current Codex session id and start a new one. The mode is chosen by the caller and passed through — the adapter does not second-guess it.

## Source

- `packages/db/src/schema/heartbeat_runs.ts` — `scheduledRetryAt`, `scheduledRetryAttempt`, `scheduledRetryReason` columns.
- `packages/db/src/migrations/0061_lively_thor_girl.sql` — migration adding the columns.
- `packages/adapters/codex-local/src/server/parse.ts` — `isCodexTransientUpstreamError`.
- `packages/adapters/codex-local/src/server/execute.ts` — `CodexTransientFallbackMode` handling.
- `server/src/__tests__/heartbeat-retry-scheduling.test.ts`, `heartbeat-process-recovery.test.ts`.
