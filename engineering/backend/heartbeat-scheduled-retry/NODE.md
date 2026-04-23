---
title: "Heartbeat Scheduled Retry"
owners: [bingran-you, cryppadotta, serenakeyitan]
soft_links: ["engineering/backend/heartbeat-run-orchestration/NODE.md", "adapters/codex-local/NODE.md"]
---

# Heartbeat Scheduled Retry

Scheduled-retry path for heartbeat runs that fail with transient upstream conditions. Distinct from process-loss recovery: instead of terminating a run on failure, the orchestrator can re-queue it for a future attempt.

**Source:** `server/src/services/heartbeat*` (scheduler + retry classification), `adapters/codex-local` (transient classifier and fallback modes), `heartbeat_runs` table.

---

## Key Decisions

### Retry State Lives on the Run Row

The `heartbeat_runs` table carries `scheduled_retry_at`, `scheduled_retry_attempt`, and `scheduled_retry_reason`. Retry intent is durable on the run itself rather than tracked out-of-band in a separate queue. The scheduler reads these fields to decide when to wake the run again, and the reason column makes the cause auditable from the run alone.

### Transient Upstream Classification Gates Retries

Automatic retries only fire when an execution failure matches a known transient upstream shape. The Codex adapter's `isCodexTransientUpstreamError` helper is the gate: it currently recognizes the remote-compaction "high demand / temporary errors" signal and similar shapes. Deterministic errors (e.g. invalid-parameter responses, account rate limits) fall through to normal failure handling so we don't mask user-actionable problems behind silent retries.

### Fallback Modes Trade Continuity for Resilience

When a retry is scheduled, the adapter can switch invocation shape via `codexTransientFallbackMode`:

- `same_session` — retry with the same session id (cheapest, preserves context).
- `safer_invocation` — same session, more conservative invocation flags.
- `fresh_session` — drop session continuity to escape a poisoned conversation.
- `fresh_session_safer_invocation` — both, for the hardest cases.

Successive attempts can escalate through these modes so a transient failure that doesn't clear with a plain retry still has a path to success.

---

## Boundaries

- **Run lifecycle and process-loss recovery** are owned by [heartbeat-run-orchestration](../heartbeat-run-orchestration/NODE.md). This node only covers the scheduled-retry path layered on top of that lifecycle.
- **Adapter-specific transient classification** lives in each adapter (today: `adapters/codex-local`). The orchestrator decides *whether* to retry based on the adapter's signal; it does not own the per-adapter error taxonomy.
- **Account-level rate limiting and quota policy** are explicitly out of scope — those should surface as failures to the operator, not be masked by retries.
