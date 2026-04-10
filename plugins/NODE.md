---
title: "Plugins"
owners: [cryppadotta, serenakeyitan]
soft_links: ["engineering/NODE.md", "engineering/backend", "product/NODE.md"]
---

# Plugins

Paperclip's extensible plugin system, enabling third-party integrations that add event handlers, scheduled jobs, webhook endpoints, UI components, and agent tools without forking the core platform.

## Sub-domains

- **[sdk/](sdk/NODE.md)** — The `@paperclipai/plugin-sdk` package: `definePlugin()` factory, `PluginContext` surface, UI bridge hooks, test harness, bundler presets, and the `create-paperclip-plugin` scaffolder.
- **[runtime/](runtime/NODE.md)** — Server-side plugin host infrastructure: lifecycle manager, worker manager, event bus, job scheduler, registry, and sandboxed execution.
- **[examples/](examples/NODE.md)** — Reference plugin implementations demonstrating SDK features from minimal to comprehensive.

## Key Architectural Decisions

- **Process isolation via JSON-RPC 2.0 over stdio.** Each plugin runs in its own child process. The host and worker communicate via newline-delimited JSON-RPC 2.0 messages on stdin/stdout. This prevents a misbehaving plugin from crashing the server and provides a clear security boundary.
- **Capability-gated access.** Plugins declare required capabilities in their manifest (`events.subscribe`, `plugin.state.read`, `issues.write`, etc.). Every worker-to-host RPC call is checked against the declared capabilities before execution. Undeclared access is rejected with `CAPABILITY_DENIED`.
- **Manifest-driven contract.** Each plugin ships a `PaperclipPluginManifestV1` declaring its identity, capabilities, entrypoints, jobs, webhooks, tools, and UI slots. The host validates the manifest at install time and uses it to wire everything together.
- **Single SDK dependency.** Plugin authors depend only on `@paperclipai/plugin-sdk`. It re-exports types from `@paperclipai/shared`, Zod for schema validation, and provides all worker-side and UI-side APIs. No direct imports of host internals are needed or permitted.
- **UI extension via sandboxed slots.** Plugin UI bundles are loaded as ES modules into host-designated slots (dashboard widgets, detail tabs, sidebar entries, settings pages, comment annotations). All UI-to-worker communication goes through a host-controlled bridge — plugins cannot access host internals directly.
- **Graceful lifecycle state machine.** Plugins move through `installed -> ready -> disabled/error -> uninstalled` with validated transitions. The host coordinates worker start/stop, emits lifecycle events, and persists state changes to the database.

## Boundaries

This domain covers the plugin SDK public API surface, the host-worker protocol contract, and example implementations. The server-side services that implement the host half of this contract live in `../engineering/backend` (route handlers, database operations). Shared types and constants flow through `@paperclipai/shared` in `../engineering/shared`.
