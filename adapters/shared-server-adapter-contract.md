---
title: "Shared Server Adapter Contract"
owners: [bingran-you, cryppadotta, serenakeyitan]
---

# Shared Server Adapter Contract

Paperclip supports multiple agent runtimes, but the control plane should not
special-case each runtime everywhere in the server and UI.

## Decision

Every runtime integrates through a shared server-side adapter contract. The
adapter owns runtime-specific execution, environment checks, skill syncing,
session serialization, quota discovery, model detection, and optional
configuration schema. The control plane consumes that contract instead of
coding directly against individual CLIs or gateways.

## Why

This keeps orchestration logic vendor-agnostic. Local CLIs, remote gateways,
and future providers can all plug into the same lifecycle without rewriting
task orchestration, approvals, cost tracking, or frontend management flows.

## Implications

- Adding a new runtime should usually mean adding one adapter package plus any
  parser or UI wiring it needs, not rewriting core orchestration.
- Runtime-specific concerns such as quota APIs, credential discovery, and skill
  installation stay inside the adapter boundary.
- Session resume semantics are normalized through shared codec and session
  management metadata, so the control plane can reason about continuity without
  knowing each runtime's native format.
- The frontend can render adapter settings from declarative config schemas
  rather than bespoke forms for every provider.

## Related Domains

- [engineering backend](../engineering/backend/NODE.md)
- [engineering frontend](../engineering/frontend/NODE.md)
