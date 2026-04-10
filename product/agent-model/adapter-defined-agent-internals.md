---
title: "Adapter-Defined Agent Internals"
owners: [bingran-you, cryppadotta, serenakeyitan]
---

# Adapter-Defined Agent Internals

Paperclip coordinates many agent runtimes, but it should not hard-code how each
runtime defines its own prompts, files, or inner-loop behavior.

## Decision

Paperclip owns the control-plane fields for an agent, while runtime-specific
identity and behavior details remain adapter-defined concerns. Files such as
agent prompts, skills, and runtime config belong to the adapter layer rather
than the core agent model.

## Why

This keeps the control plane vendor-agnostic and allows local CLIs, remote
gateways, and future runtimes to participate without forcing one universal
execution model onto all of them.

## Implications

- Adding a new runtime should usually extend adapter config and capability
  surfaces, not the core `Agent` concept.
- Paperclip does not need to understand the semantics of `CLAUDE.md`,
  `SOUL.md`, heartbeat instructions, or provider-specific prompt files in order
  to orchestrate the agent.
- Product comparisons between runtimes should be expressed as adapter
  capabilities and constraints, not as divergent core agent types.
- Control-plane status, assignment, org structure, and governance remain
  stable even when agent internals vary widely.

## Related Domains

- [adapters](../../adapters/NODE.md)
- [product company model](../company-model/NODE.md)
- [engineering backend](../../engineering/backend/NODE.md)
