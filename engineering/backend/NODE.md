---
title: "Backend Server"
owners: [bingran-you, cryppadotta, serenakeyitan]
soft_links: ["engineering/database", "engineering/shared", "engineering/frontend", "engineering/cli/NODE.md", "adapters", "adapters/claude-local/NODE.md", "adapters/codex-local/NODE.md", "adapters/cursor-local/NODE.md", "adapters/gemini-local/NODE.md", "adapters/openclaw-gateway/NODE.md", "adapters/opencode-local/NODE.md", "adapters/pi-local/NODE.md", "plugins", "plugins/runtime/NODE.md"]
---

# Backend Server

Express-based HTTP server and real-time event system. Source: `/server`.

Package: `@paperclipai/server`

## Architecture

### Route / Service Split

Routes (`/server/src/routes/`) are thin HTTP handlers — parse request, call service, return response. Services (`/server/src/services/`) contain all business logic. This keeps routes testable and services reusable (e.g., the CLI can call service logic directly for heartbeat runs).

Key route groups:
- **companies, agents, projects, issues, goals** — core domain CRUD
- **routines** — scheduled/triggered automation definitions
- **approvals** — human-in-the-loop governance gates
- **execution-workspaces** — sandboxed environments where agents run
- **costs, dashboard, sidebar-badges** — operational visibility
- **plugins, adapters** — extension management
- **secrets** — credential vault (supports multiple secret providers)
- **llms** — LLM provider configuration
- **assets** — file/attachment storage

### Authentication & Authorization

- **BetterAuth** (`/server/src/auth/better-auth.ts`) handles session-based authentication with email/password.
- Three deployment modes: `local` (no auth), `authenticated` (BetterAuth sessions), and `board` (public read-only boards with claim tokens).
- The `actorMiddleware` resolves the current user/agent identity on every request.
- `boardMutationGuard` prevents writes on public board endpoints.
- `privateHostnameGuard` restricts access by hostname in exposed deployments.

### Real-time

WebSocket-based live events (`/server/src/realtime/live-events-ws.ts`) push updates to connected UI clients for issue status changes, agent activity, etc.

### Plugin Host Runtime

The server is a full plugin host. Key subsystems:
- **Plugin loader** — discovers and loads plugin packages from a local directory
- **Plugin lifecycle manager** — init, start, stop, health checks
- **Plugin worker manager** — isolates plugin execution
- **Plugin job scheduler/coordinator** — cron and event-triggered plugin jobs
- **Plugin event bus** — decoupled event propagation between core and plugins
- **Plugin tool dispatcher/registry** — plugins expose tools that agents can invoke

### Storage

Pluggable storage provider system (`/server/src/storage/`): local disk and S3 backends, selected via config. Used for assets, attachments, and exports.

### Configuration

Config is loaded from environment variables, `.env` files, and a YAML config file (`~/.paperclip/config.yaml`). The config schema defines deployment mode, exposure, database mode (embedded vs external Postgres), auth settings, storage provider, and secret provider.

## Key Decisions

- **No DI framework.** Services are plain functions/modules. Dependencies (db, config) are passed explicitly.
- **Drizzle queries live in services, not a separate "repository" layer.** This keeps the codebase flat and avoids over-abstraction.
- **Agent execution is adapter-driven.** The server never calls LLM APIs directly — it delegates to adapter packages that implement a standard interface.
- **Secrets are provider-abstracted.** Supports file-based, environment-based, and external secret providers so the same server code works in local dev and cloud deployments.

## Sub-domains

- [dev-runner/](dev-runner/) — Local development runner and worktree dev tooling
- [heartbeat-run-orchestration/](heartbeat-run-orchestration/) — Run lifecycle state machine and process recovery
- [instance-database-backups/](instance-database-backups/) — Instance-admin HTTP trigger for on-demand database backups
- [static-asset-serving/](static-asset-serving/) — Static asset cache headers and SPA fallback routing

## Decision Records

- [shared-api-and-actor-scoped-authorization.md](shared-api-and-actor-scoped-authorization.md) — Why board and agent traffic share one API surface, with authorization derived from a common request actor model.
