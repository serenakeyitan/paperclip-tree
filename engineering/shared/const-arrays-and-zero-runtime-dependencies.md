---
title: "Const Arrays And Zero Runtime Dependencies"
owners: [cryppadotta, serenakeyitan]
---

# Const Arrays And Zero Runtime Dependencies

The shared package is the contract layer that every major Paperclip package can
import safely. That only works if it stays small, portable, and runtime-light.

## Decision

Model shared domain vocabulary with `as const` arrays plus exported types and
validators, and keep `@paperclipai/shared` free of third-party runtime
dependencies.

## Why

This gives Paperclip one canonical definition of statuses, kinds, schema
shapes, and configuration contracts that can be used by backend, frontend, CLI,
database code, and adapters without bundle bloat or dependency cycles.

## Implications

- New cross-package enums, statuses, and request/response shapes should be
  introduced here before being duplicated elsewhere.
- Shared should hold contracts and validation helpers, not business logic that
  depends on server or UI internals.
- Importing shared remains safe in build scripts, browser bundles, adapters,
  and command-line entrypoints.
- Const-array patterns win over TypeScript enums because they support both
  runtime iteration and type inference from the same source of truth.

## Related Domains

- [engineering backend](../backend/NODE.md)
- [engineering frontend](../frontend/NODE.md)
- [engineering cli](../cli/NODE.md)
- [plugins sdk](../../plugins/sdk/NODE.md)
