---
title: "Transient Upstream Error Recovery"
owners: [bingran-you, cryppadotta, serenakeyitan]
soft_links: [adapters/claude-local/NODE.md, adapters/codex-local/NODE.md, engineering/backend/NODE.md, engineering/backend/heartbeat-run-orchestration/NODE.md]
---

# Transient Upstream Error Recovery

Paperclip distinguishes **transient upstream failures** (rate limits, overload, usage-limit windows, 429/503/529, "try again later") from deterministic adapter errors so the heartbeat loop can retry intelligently instead of marking the run failed.

## Key Decisions

### Shared `errorFamily` on AdapterExecutionResult

`AdapterExecutionResult` carries an optional `errorFamily` discriminator (currently the single value `"transient_upstream"`) alongside `errorCode`, plus an optional ISO `retryNotBefore` timestamp. Each adapter classifies its own upstream failures using adapter-specific regexes (e.g. `claude_transient_upstream`, `codex_transient_upstream`) and maps them onto the shared family so the server can react uniformly without knowing adapter internals.

### Adapter-Specific Detection, Server-Side Scheduling

Claude and Codex local adapters own the parsing: they detect rate-limit / overload / usage-cap phrasing (including "out of extra usage", "you've hit your usage limit", "resets at 4pm", "try again at 11:31 PM") in stdout, stderr, and parsed error payloads, and extract a concrete retry-after time when the upstream provides one. The heartbeat service consumes `errorFamily` and `retryNotBefore` to schedule the next attempt, rather than burning a retry immediately.

### Retry Time Extraction

When upstream messages include a reset/retry hint ("resets 4pm (America/Chicago)", "try again at 11:31 PM"), the adapter parses the wall-clock time, applies any timezone hint, and returns a `retryNotBefore` Date. Without an explicit hint, `retryNotBefore` is null and the heartbeat falls back to its default backoff.

## Why This Matters

Before this contract, transient provider outages and subscription-window resets looked identical to deterministic errors and consumed retry budget or surfaced as failed runs. Treating them as a typed family lets governance, heartbeat scheduling, and UI all reason about "will succeed later" vs "will not succeed without intervention".
