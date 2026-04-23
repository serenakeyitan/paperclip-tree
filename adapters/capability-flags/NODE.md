---
title: "Adapter Capability Flags"
owners: [bingran-you, cryppadotta, serenakeyitan]
soft_links: ["adapters", "engineering/backend/NODE.md", "engineering/frontend/NODE.md", "product/agent-model/adapter-defined-agent-internals.md"]
---

# Adapter Capability Flags

Adapters declare which Paperclip-managed runtime features they support through optional capability fields on `ServerAdapterModule`. The server and UI consult these flags instead of maintaining hardcoded adapter-type allowlists and denylists for "local" behaviors.

**Source:** `packages/adapter-utils/src/types.ts`, `server/src/routes/adapters.ts`, `server/src/routes/agents.ts`, `ui/src/adapters/use-adapter-capabilities.ts`

---

## Decision

Paperclip models adapter-specific local behaviors as adapter-declared capabilities, not as special cases attached to a fixed set of built-in adapter type names.

## Why

External adapter plugins previously could not participate in local-adapter features such as managed instructions bundles, runtime-skill syncing, or local JWT auth unless Paperclip source code was updated to add their type to multiple server and UI lists. Capability flags let adapters opt into those behaviors through the shared adapter contract.

## Capability Surface

- **`supportsLocalAgentJwt`** — whether heartbeat should mint a local JWT for runs using the adapter.
- **`supportsInstructionsBundle`** — whether the adapter supports the managed instructions bundle workflow and corresponding UI editor.
- **`instructionsPathKey`** — which `adapterConfig` key stores the instructions file path when instructions bundles are supported.
- **`requiresMaterializedRuntimeSkills`** — whether Paperclip must write runtime skill entries to disk before execution instead of passing them only through config.
- **Derived `supportsSkills`** — computed server-side from `listSkills` or `syncSkills` and exposed alongside the explicit flags.

## API Contract

`GET /api/adapters` returns a `capabilities` object for each adapter entry. The frontend caches this through a dedicated capability hook and uses it to decide whether to show local-only affordances such as the instructions bundle editor, skill-management UI, and legacy working-directory behavior.

## Compatibility Rules

When an adapter does not set capability flags, the server keeps backward compatibility by falling back to legacy hardcoded maps and sets for built-in adapter types. External adapters that omit the flags default to `false` and therefore do not receive these local-only features until they opt in explicitly.

Built-in adapters are expected to declare their flags directly so the contract becomes self-describing over time instead of depending on the fallback path.

## Implications

- Adding a new adapter usually means declaring its capabilities in the adapter module, not editing core server or UI lists.
- "Local" is treated as a bundle of specific capabilities rather than a permanent adapter category.
- Product differences between runtimes stay adapter-defined, which keeps the control plane extensible for plugin adapters and future runtimes.

## Related Domains

- [adapters](../NODE.md)
- [engineering backend](../../engineering/backend/NODE.md)
- [engineering frontend](../../engineering/frontend/NODE.md)
- [product agent model](../../product/agent-model/NODE.md)
