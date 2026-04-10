---
title: "Plugin Examples"
owners: [bingran-you, cryppadotta, serenakeyitan]
soft_links: ["plugins/sdk"]
---

# Plugin Examples

Reference plugin implementations in `packages/plugins/examples/` that demonstrate SDK features at varying levels of complexity. These serve as both documentation and integration tests for the plugin system.

## Purpose

Provide copy-able starting points for plugin authors and serve as living documentation of the SDK's capabilities. Each example targets a different slice of the API surface.

## Examples

- **plugin-hello-world-example** — Minimal plugin. Defines `setup()` and `onHealth()` only. Demonstrates the smallest viable plugin: the `definePlugin()` + `runWorker()` entrypoint pattern with no event handlers, jobs, or UI.
- **plugin-file-browser-example** — Workspace-aware plugin. Registers multiple `data` handlers (`fileList`, `fileContent`, `workspaces`, `comment-file-links`) and a `writeFile` action. Demonstrates reading project workspaces via `ctx.projects.listWorkspaces()`, filesystem access, and comment body parsing for file path extraction.
- **plugin-authoring-smoke-example** — Test-focused plugin with a Vitest spec (`tests/plugin.spec.ts`). Demonstrates using `createTestHarness()` from `@paperclipai/plugin-sdk/testing` to unit-test plugin handlers without a running server.
- **plugin-kitchen-sink-example** — Comprehensive reference exercising most SDK features: events, jobs, webhooks, data handlers, actions, tools, streams, launchers, state management across all scope kinds, entity CRUD, secrets resolution, HTTP fetching, process spawning, and health diagnostics with detailed status reporting.

## Key Decisions

- **Examples live in-tree, not in a separate repo.** They are part of the pnpm workspace and import `@paperclipai/plugin-sdk` via `workspace:*`. This ensures they always compile against the current SDK version.
- **Graduated complexity.** Hello World shows the minimum, File Browser shows a realistic integration, Kitchen Sink shows the full surface. Authors pick the closest starting point.

## Decision Records

- [examples-live-in-tree-as-sdk-contract-tests.md](examples-live-in-tree-as-sdk-contract-tests.md) — Why example plugins are kept in-workspace as both docs and compatibility coverage.

## Boundaries

Examples demonstrate SDK usage patterns. They do not define the API surface (that is `../sdk`) or implement host-side behavior (that is `../runtime`).
