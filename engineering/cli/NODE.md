---
title: "CLI"
owners: [cryppadotta, serenakeyitan]
soft_links: ["engineering/backend", "engineering/shared", "engineering/database", "adapters/NODE.md"]
---

# CLI

Command-line interface for managing Paperclip instances. Source: `/cli`.

Package: `@paperclipai/cli` (published as `paperclipai` on npm)

## Stack

- **Commander.js** for command parsing and help generation
- **tsx** for TypeScript execution during development
- Built with **esbuild** for npm distribution

## Command Structure

### Instance Management
- **`onboard`** — interactive first-time setup wizard
- **`configure`** — modify instance configuration
- **`doctor`** — diagnose common setup problems
- **`env`** — inspect and manage environment variables
- **`run`** — start the Paperclip server
- **`allowed-hostname`** — manage hostname allowlist for exposed deployments
- **`db-backup`** — trigger database backup

### Agent Operations
- **`heartbeat-run`** — manually trigger an agent heartbeat cycle
- **`auth-bootstrap-ceo`** — bootstrap the first CEO user with admin access
- **`worktree`** — manage git worktree-based execution workspaces

### API Client Commands (`/src/commands/client/`)
A set of subcommands that call the Paperclip HTTP API for programmatic control:
- `company` — list/create/manage companies
- `agent` — list/create/manage agents
- `issue` — create/list/update issues
- `approval` — list/resolve approvals
- `activity` — query activity log
- `dashboard` — fetch dashboard data
- `context` — manage agent context
- `feedback` — submit/query feedback
- `plugin` — manage plugins
- `auth` — authentication operations
- `routines` — manage routine definitions

### Adapter & Check Integration
- `/src/adapters/` — CLI-side adapter configuration and discovery
- `/src/checks/` — health check definitions run by `doctor`

## Key Decisions

- **Commander.js, not oclif or yargs.** Keeps the CLI lightweight with minimal dependencies.
- **Client commands mirror the REST API.** Every major API resource has a corresponding CLI subcommand, making the CLI a complete alternative to the web UI for automation.
- **Data directory isolation** (`--data-dir` flag). Allows running multiple isolated Paperclip instances on the same machine by overriding the default `~/.paperclip` data root.
- **Telemetry is opt-in** and shares the same event schema as the server (via `@paperclipai/shared`).

## Decision Records

- [cli-mirrors-api-and-instance-ops.md](cli-mirrors-api-and-instance-ops.md) — Why the CLI is both the local instance operator surface and the resource-oriented API client.
