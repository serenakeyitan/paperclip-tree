---
title: "Codex Transient Upstream Retries"
owners: [bingran-you, cryppadotta, serenakeyitan]
soft_links: [adapters/codex-local/NODE.md, adapters/codex-local/fast-mode/NODE.md, engineering/backend/heartbeat-run-orchestration/NODE.md]
---

# Codex Transient Upstream Retries

The Codex local adapter detects **transient upstream failures** from the Codex CLI and chooses a fallback strategy so Paperclip can retry without losing session continuity unnecessarily.

## Key Decisions

### Narrow Classification

`isCodexTransientUpstreamError` (in `packages/adapters/codex-local/src/server/parse.ts`) only classifies a failure as transient when the output matches both a transient signal (high demand, temporary errors, 429, server overloaded, service unavailable, try again later) **and** a known shape — currently the remote-compaction high-demand failure or an explicit "high demand / temporary errors" message. Generic 429s are intentionally excluded because they may reflect user/account quota rather than upstream weather.

### Four Fallback Modes

`codexTransientFallbackMode` on the execution context selects how the adapter retries:

- `same_session` — retry within the existing Codex session.
- `safer_invocation` — retry with a more conservative invocation shape.
- `fresh_session` — start a new Codex session for the retry.
- `fresh_session_safer_invocation` — combine a fresh session with the safer invocation.

`fallbackModeUsesSaferInvocation` centralizes which modes imply the safer invocation, so call sites do not duplicate the predicate.

### Deterministic Errors Stay Terminal

Deterministic failures (e.g. `Unknown parameter: 'prompt_cache_retention'`) are not classified as transient and therefore are never auto-retried, even if they surface from the same `remote compact task` path.

## Cross-Domain

Classification is local to the adapter; the actual scheduling of the retry attempt is owned by the heartbeat run orchestrator and recorded via `scheduled_retry_*` columns on `heartbeat_runs`.
