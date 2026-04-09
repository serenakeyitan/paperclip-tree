---
title: "Plugin SDK"
owners: [cryppadotta, serenakeyitan]
soft_links: ["plugins/runtime", "engineering/shared"]
---

# Plugin SDK

The `@paperclipai/plugin-sdk` package (`packages/plugins/sdk/`) — the stable public API surface for authoring Paperclip plugins. Published as a single dependency that provides everything a plugin author needs: worker-side context, UI bridge hooks, test harness, bundler presets, and JSON-RPC protocol types.

## Purpose

Give third-party developers a clean, typed, versioned API for extending Paperclip without coupling to host internals. The SDK is the only package plugin authors import; it re-exports shared types and Zod so they have zero additional dependencies.

## Key Components

- **`definePlugin()`** — Top-level factory. Authors pass a `PluginDefinition` with a required `setup(ctx)` hook and optional lifecycle hooks (`onHealth`, `onConfigChanged`, `onShutdown`, `onValidateConfig`, `onWebhook`). Returns a sealed `PaperclipPlugin` object.
- **`PluginContext`** — The rich context object provided to `setup()`. Exposes typed clients for: `config`, `events`, `jobs`, `state`, `entities`, `data`, `actions`, `tools`, `streams`, `launchers`, `http`, `secrets`, `activity`, `metrics`, `telemetry`, `logger`, and read access to `companies`, `projects`, `issues`, `agents`, `goals`.
- **`runWorker()`** — Bootstraps the worker-side RPC host that listens on stdin for JSON-RPC requests, dispatches to plugin handlers, and sends responses on stdout. This is the runtime glue between the SDK and the host process.
- **UI hooks** (`@paperclipai/plugin-sdk/ui`) — React hooks (`usePluginData`, `usePluginAction`, `usePluginStream`, `usePluginToast`, `useHostContext`) that plugin UI components use to communicate with the worker through the host bridge.
- **Test harness** (`@paperclipai/plugin-sdk/testing`) — In-memory `createTestHarness()` that provides a fake `PluginContext` with seedable entities, allowing plugin authors to unit-test handlers without running a server.
- **Bundler presets** (`@paperclipai/plugin-sdk/bundlers`) — Pre-configured esbuild and Rollup options for building worker, manifest, and UI bundles.
- **Dev server** (`@paperclipai/plugin-sdk/dev-server`) — Local HTTP server for hot-reload UI development at port 4177.
- **Scaffolder** (`create-paperclip-plugin`) — CLI tool that generates a complete plugin starter project with manifest, worker, UI, tests, and build configs.

## Key Decisions

- **Worker entrypoint pattern.** Plugins export `definePlugin({...})` as default and call `runWorker(plugin, import.meta.url)` at module scope. This dual-export allows both the host to import the definition and the process to self-start as a worker.
- **JSON-RPC 2.0 as the wire protocol.** All host-worker communication uses standard JSON-RPC 2.0 over NDJSON on stdio. This is language-agnostic in principle, though the current SDK targets TypeScript/Node.js.
- **Bidirectional RPC.** The host calls the worker (initialize, health, shutdown, onEvent, runJob, getData, performAction, executeTool) and the worker calls the host (state.get/set, entities.upsert, events.emit, http.fetch, secrets.resolve, etc.). Both directions use the same protocol.
- **UI slot isolation.** Plugin UI bundles are loaded into named slots (dashboardWidget, detailTab, sidebar, projectSidebarItem, commentAnnotation, commentContextMenuItem, settingsPage, page). Props always include a `PluginHostContext` with current company/project/entity IDs.
- **Zod re-exported.** The SDK re-exports Zod so plugin authors can define config schemas and tool parameter schemas without adding a separate dependency.

## Boundaries

The SDK defines the contract; it does not implement the host side. Host-side service implementations (worker manager, event bus, job scheduler) live in `../runtime`. Shared type definitions and constants originate in `../../engineering/shared` and are re-exported through this SDK.
