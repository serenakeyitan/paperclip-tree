---
title: "Plugin Runtime"
owners: [bingran-you, cryppadotta, serenakeyitan]
soft_links: ["plugins/sdk", "engineering/backend", "engineering/database"]
---

# Plugin Runtime

The server-side plugin host infrastructure (`server/src/services/plugin-*.ts`) — the services that load, run, monitor, and manage plugin worker processes. This is the host half of the JSON-RPC protocol contract defined in the SDK.

## Purpose

Provide a secure, observable, and resilient execution environment for third-party plugin code. The runtime manages the full plugin lifecycle from installation to uninstallation, coordinates worker processes, routes events and jobs, and enforces capability boundaries.

## Key Services

- **PluginLifecycleManager** — State-machine controller for plugin status transitions (`installed -> ready -> disabled/error -> uninstalled`). Coordinates worker start/stop, emits lifecycle events, persists state.
- **PluginWorkerManager** — Spawns and manages child processes for each active plugin. Handles JSON-RPC message routing between host and worker, timeout enforcement, and crash recovery.
- **PluginRegistry** — CRUD operations for the `plugins`, `plugin_config`, `plugin_entities`, `plugin_jobs`, and related database tables via Drizzle ORM. The persistence layer for all plugin metadata.
- **PluginLoader** — Resolves plugin packages (from npm, local paths, or monorepo workspace), reads manifests, validates structure, and prepares plugins for installation.
- **PluginManifestValidator** — Validates `PaperclipPluginManifestV1` at install time against the expected schema and capability rules.
- **PluginEventBus** — Routes domain events (issue.created, agent.status_changed, etc.) to plugins that have subscribed via `ctx.events.on()`.
- **PluginJobScheduler / PluginJobCoordinator / PluginJobStore** — Manages scheduled and on-demand job execution. The coordinator dispatches jobs to worker processes; the store persists job definitions and run history.
- **PluginHostServices** — Implements the host side of every `WorkerToHostMethods` RPC call (state.get, entities.upsert, secrets.resolve, etc.), with capability gating via the SDK's `getRequiredCapability()` mapping.
- **PluginToolRegistry / PluginToolDispatcher** — Registers tools declared in plugin manifests and dispatches `executeTool` calls from agents to the appropriate plugin worker.
- **PluginStreamBus** — Fans out real-time stream events from workers to connected SSE clients.
- **PluginSecretsHandler** — Resolves secret references to actual values, keeping sensitive data out of plugin config.
- **PluginConfigValidator** — Validates operator-supplied plugin configuration against the manifest's `instanceConfigSchema`.
- **PluginRuntimeSandbox** — Security boundaries for plugin worker processes.
- **PluginDevWatcher** — Watches local plugin directories for changes during development and triggers hot reloads.
- **PluginLogRetention / PluginHostServiceCleanup** — Housekeeping: log rotation and cleanup of orphaned host-side resources.

## Key Decisions

- **One process per plugin.** Each active plugin gets its own Node.js child process. Isolation prevents a failing plugin from taking down the server or other plugins. The trade-off is higher memory overhead per plugin.
- **Capability enforcement at the host boundary.** Every worker-to-host RPC call is checked against the plugin's declared capabilities before the host service executes. This is the single enforcement point — the SDK clients do not do their own checks.
- **Graceful shutdown with deadline.** When stopping a plugin, the host sends a `shutdown` RPC and waits up to 10 seconds (configurable) for the worker to finish in-flight work. After the deadline: SIGTERM, then SIGKILL.
- **Event-driven coordination.** The lifecycle manager emits events (`plugin.loaded`, `plugin.enabled`, etc.) that other services listen to. The job coordinator pauses jobs when a plugin moves out of `ready` state. The tool dispatcher deregisters tools when a plugin is disabled.
- **Database-backed state.** Plugin state, entities, job runs, and webhook deliveries are persisted in PostgreSQL tables with `pluginId` scoping, not in-memory. This survives server restarts.

## Decision Records

- [worker-isolation-and-capability-gates.md](worker-isolation-and-capability-gates.md) — Why plugin workers are isolated out of process and why the host enforces declared capabilities at install-time and runtime.

## Boundaries

This domain covers the host-side services that implement the plugin execution contract. The contract itself (types, protocol, SDK API) is defined in `../sdk`. Route handlers that expose plugin management over HTTP live in `../../engineering/backend`. Database schema and migrations live in `../../engineering/database`.
