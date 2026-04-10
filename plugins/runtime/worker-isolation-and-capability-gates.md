---
title: "Worker Isolation And Capability Gates"
owners: [bingran-you, cryppadotta, serenakeyitan]
---

# Worker Isolation And Capability Gates

Paperclip's plugin runtime is intentionally not an in-process extension model.
The host treats plugin execution as a separate boundary that needs both failure
isolation and explicit permission checks.

## Decision

Run each active plugin in its own worker process and enforce plugin
capabilities at the host boundary, both when a plugin is installed and when a
worker calls back into host services at runtime.

## Why

This keeps a plugin crash, hang, or bad dependency from taking down the core
server, and it keeps host-side power concentrated in one place instead of
trusting every plugin call path to self-police. The runtime can therefore
support richer extensions without collapsing the control plane into a shared
process and permission model.

## Implications

- The host-worker protocol is a real operational boundary, not just an
  implementation detail. Timeouts, restart policy, and graceful shutdown are
  part of the product contract.
- Capability checks have to happen where the host receives an operation, so the
  same rules apply regardless of which plugin SDK helpers or custom code paths
  produced the request.
- Plugin authors can build substantial integrations, but they do so through a
  constrained bridge rather than direct access to host internals.
- Expanding the plugin surface usually means defining new host operations and
  their capability mapping, not bypassing the gate.

## Related Domains

- [plugin sdk](../sdk/NODE.md)
- [engineering backend](../../engineering/backend/NODE.md)
- [engineering database](../../engineering/database/NODE.md)
