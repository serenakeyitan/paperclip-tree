---
title: "Capability Flags"
owners: [bingran-you, cryppadotta, serenakeyitan, cpfarhood]
---

Adapters declare structured **capability flags** on the `ServerAdapterModule` interface that describe what features each adapter supports at runtime. Unlike `getConfigSchema` (which defines adapter-specific settings UI) or `ADAPTER_SESSION_MANAGEMENT` (which only covers context management), capability flags are a general-purpose mechanism for any boolean or enum capability.

**Source:** `packages/adapter-utils/src/types.ts` (type definitions), `server/src/adapters/registry.ts` (registry exposes capabilities), `server/src/routes/adapters.ts` (API endpoint).

## Architecture

Capability flags flow through three layers:

1. **Declaration** — Each adapter's `ServerAdapterModule` declares its capabilities as structured flags in `adapter-utils/types.ts`. These cover features like session resume support, native context management, skill sync, and fast mode.
2. **Registry & API** — The adapter registry aggregates capability flags and exposes them through a dedicated adapters API route. This allows any consumer (frontend, CLI, external agents via MCP) to query what a given adapter supports.
3. **UI consumption** — A React hook (`use-adapter-capabilities.ts`) queries the capabilities API and drives conditional rendering across `AgentConfigForm`, `OnboardingWizard`, `AdapterManager`, and `AgentDetail`. Config options that don't apply to the selected adapter are hidden rather than shown-and-disabled.

## Key Decisions

- **Capability flags on the module interface** rather than a separate config file or database table. Capabilities are inherent to the adapter implementation, so they live next to the code that implements them.
- **Conditional UI rendering** driven by capabilities rather than adapter-type checks. The frontend never hardcodes adapter names — it queries capability flags, so new adapters get correct UI behavior automatically.
- **Adapter authoring docs updated** (`docs/adapters/creating-an-adapter.md`) to guide new adapter authors on declaring capability flags.
