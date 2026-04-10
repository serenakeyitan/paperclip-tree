---
title: "Minimal Session State And Cached Model Discovery"
owners: [bingran-you, cryppadotta, serenakeyitan]
---

# Minimal Session State And Cached Model Discovery

Pi exposes a simpler session model than some other runtimes, but it still needs
to participate cleanly in Paperclip's shared execution and configuration flows.

## Decision

Keep Pi session serialization intentionally minimal and pair its dynamic model
discovery with explicit cached and uncached code paths so callers can choose
freshness or speed knowingly.

## Why

A smaller session contract matches the runtime's actual capabilities better
than forcing extra metadata into the codec, and explicit model-discovery
caching keeps adapter behavior understandable in both execution and test paths.

## Implications

- The adapter normalizes Pi's `session` field variant without pretending Pi
  supports the same richer session shape as other local runtimes.
- Model availability can be surfaced dynamically while still avoiding repeated
  expensive discovery work when cached results are acceptable.
- Test code can control cache state directly instead of relying on implicit
  process-global behavior.
- Pi-specific operational differences stay adapter-local while the server still
  consumes the same high-level adapter contract.

## Related Domains

- [adapters](../NODE.md)
- [engineering backend](../../engineering/backend/NODE.md)
- [product agent model](../../product/agent-model/NODE.md)
