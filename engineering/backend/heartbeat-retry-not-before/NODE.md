---
title: "Heartbeat Retry-Not-Before Scheduling"
owners: [bingran-you, cryppadotta, serenakeyitan]
soft_links: [adapters/claude-local/NODE.md, adapters/codex-local/NODE.md, engineering/backend/heartbeat-run-orchestration/NODE.md, engineering/backend/transient-upstream-recovery/NODE.md]
---

# Heartbeat Retry-Not-Before Scheduling

When an adapter classifies a failure as `errorFamily: "transient_upstream"`, it may also return a `retryNotBefore` ISO timestamp on `AdapterExecutionResult`. The heartbeat loop honors this hint and defers the next retry of the run until at least that wall-clock time, instead of using its default backoff schedule.

## Key Decisions

### Adapters Own the Retry Window

The adapter, not the heartbeat scheduler, is responsible for parsing provider-specific retry hints out of error text. Codex extracts the "try again at 11:31 PM" timestamp from usage-limit messages (with optional timezone hints in parentheses) and Claude extracts the equivalent "resets at …" line from the subscription-window failure. Each adapter normalizes the parsed value into an absolute ISO timestamp before returning it.

**Rationale:** Provider error formats are unstable and adapter-specific. Centralizing the regex in the heartbeat would couple core orchestration to every upstream's prose; keeping it in the adapter parser modules (`packages/adapters/*/src/server/parse.ts`) lets each adapter evolve independently.

### Scheduler Treats `retryNotBefore` as a Floor, Not an Exact Time

The heartbeat loop uses `retryNotBefore` as a lower bound on when the next attempt may run; normal backoff still applies on top. This means a transient failure with no `retryNotBefore` falls back to the existing retry policy, and a `retryNotBefore` in the past collapses to the default schedule.

### Pairs With `errorFamily: "transient_upstream"`

`retryNotBefore` is only meaningful when the failure is also classified as transient upstream. Deterministic adapter errors (compaction failures, unknown session, auth required) must not set `retryNotBefore` — the heartbeat loop will not retry them on a schedule regardless.

## Why This Exists

Without `retryNotBefore`, the heartbeat would either (a) burn retries against an upstream that has explicitly told us to wait an hour, or (b) over-correct with a fixed long backoff that wastes time when the window is short. The hint lets each provider tell Paperclip exactly when to come back.
