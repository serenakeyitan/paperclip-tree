---
title: "Dynamic Model Discovery From Runtime Config"
owners: [cryppadotta, serenakeyitan]
---

# Dynamic Model Discovery From Runtime Config

OpenCode can sit on top of different providers and locally configured model
sets, so a fixed Paperclip-side model list would drift quickly from reality.

## Decision

Discover OpenCode models dynamically from the runtime's own configuration and
validate configured availability at execution time instead of hardcoding a
provider/model matrix inside Paperclip.

## Why

This keeps Paperclip aligned with whatever providers and models the local
OpenCode installation actually exposes, while catching misconfiguration before
an agent run fails in a confusing way.

## Implications

- The adapter owns OpenCode config parsing and model-list freshness concerns.
- Paperclip can present operator-facing model choices that reflect the real
  local runtime instead of a stale baked-in list.
- Misaligned Paperclip agent settings fail early through explicit validation.
- OpenCode remains extensible across providers without forcing core backend or
  frontend changes whenever provider support evolves.

## Related Domains

- [adapters](../NODE.md)
- [engineering backend](../../engineering/backend/NODE.md)
- [engineering frontend](../../engineering/frontend/NODE.md)
