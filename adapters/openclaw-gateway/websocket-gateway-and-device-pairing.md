---
title: "WebSocket Gateway And Device Pairing"
owners: [cryppadotta, serenakeyitan]
---

# WebSocket Gateway And Device Pairing

OpenClaw is the clearest case where Paperclip is integrating with a remote
runtime boundary rather than spawning a local coding CLI.

## Decision

Integrate OpenClaw through a WebSocket gateway protocol with device identity,
pairing, and session-key strategies instead of treating it like a local
child-process adapter.

## Why

The OpenClaw model depends on bidirectional event streaming, remote challenge
flows, and device authorization semantics that do not map cleanly onto a
start-process-read-stdout pattern.

## Implications

- Session routing is a protocol choice (`issue`, `fixed`, `run`), not just a
  local process resume token.
- Skill injection is intentionally absent because remote OpenClaw workers own
  their capabilities on the other side of the gateway.
- The adapter's operational surface centers on connectivity, pairing, and
  auth configuration rather than local binary installation.
- Gateway adapters remain first-class citizens in the shared adapter model even
  though they do not share every local-adapter capability.

## Related Domains

- [adapters](../NODE.md)
- [engineering backend](../../engineering/backend/NODE.md)
- [product agent model](../../product/agent-model/NODE.md)
