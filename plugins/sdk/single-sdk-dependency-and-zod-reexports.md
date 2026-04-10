---
title: "Single SDK Dependency And Zod Re-Exports"
owners: [bingran-you, cryppadotta, serenakeyitan]
---

# Single SDK Dependency And Zod Re-Exports

Paperclip's plugin ecosystem is easier to adopt when authors do not need to
understand the host's package graph in order to build an extension.

## Decision

Expose plugin authoring through a single public package,
`@paperclipai/plugin-sdk`, and re-export the shared contract types plus Zod
from that package instead of asking authors to import multiple internal
packages directly.

## Why

This creates a narrow, stable public surface for plugin development and keeps
host internals replaceable without turning every plugin into a tightly coupled
workspace consumer.

## Implications

- New plugin-facing capabilities should be added to the SDK instead of telling
  authors to reach into host-only packages.
- Examples and scaffolding should compile against the SDK as the canonical
  authoring surface.
- Shared types can evolve behind the SDK boundary without requiring plugin
  authors to track internal package topology.
- Config schemas and tool parameter schemas can use Zod without forcing a
  second dependency decision on every plugin author.

## Related Domains

- [plugins runtime](../runtime/NODE.md)
- [plugins examples](../examples/NODE.md)
- [engineering shared](../../engineering/shared/NODE.md)
