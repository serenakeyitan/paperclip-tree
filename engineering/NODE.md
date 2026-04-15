---
title: "Engineering"
owners: [bingran-you, cryppadotta, serenakeyitan]
soft_links: ["adapters", "plugins", "infrastructure", "infrastructure/deployment/NODE.md", "infrastructure/testing/NODE.md", "product/NODE.md", "product/agent-model/NODE.md", "product/company-model/NODE.md", "product/governance/NODE.md", "product/task-system/NODE.md"]
---

# Engineering

Technical architecture and implementation of the Paperclip orchestration platform. Paperclip is a pnpm workspace monorepo (`server`, `ui`, `cli`, `packages/*`) built on Node.js/Express + React, backed by PostgreSQL via Drizzle ORM.

## Sub-domains

- **[backend/](backend/NODE.md)** — Express server: routes, services, middleware, auth, real-time events, plugin host runtime.
- **[frontend/](frontend/NODE.md)** — React 19 SPA: board UI, agent management, issue tracking, approval workflows.
- **[database/](database/NODE.md)** — Drizzle ORM schema, migrations, embedded Postgres support, backup/restore.
- **[shared/](shared/NODE.md)** — Cross-package types, constants, validators, and config schemas.
- **[cli/](cli/NODE.md)** — `paperclipai` CLI for setup, diagnostics, worktree management, and API client operations.
- **[mcp/](mcp/NODE.md)** — Standalone MCP server (`@paperclipai/mcp-server`) exposing Paperclip capabilities as tools for MCP-compatible AI clients.
- **[contributor-guide/](contributor-guide/NODE.md)** — Developer setup, contribution workflows, and project conventions.
- **[mcp-server/](mcp-server/NODE.md)** — Standalone MCP server package: tool surface enumeration, transport modes, and auth design.

## Key Architectural Decisions

- **Monorepo with pnpm workspaces.** All packages, server, UI, and CLI live in one repo. Internal packages use `workspace:*` protocol.
- **Service-oriented backend.** The Express server separates concerns into route handlers (thin HTTP layer) and service modules (business logic). Services are standalone `.ts` files, not class-based DI — they accept `db` and config as function arguments.
- **Multi-company isolation.** A single Paperclip instance can host multiple "companies" (autonomous AI orgs). Data is scoped by `companyId` at the query level, not by separate databases.
- **Plugin system is first-class.** The server hosts a full plugin runtime: loader, lifecycle manager, worker manager, job scheduler, event bus, and sandboxed execution. Plugins extend Paperclip without forking.
- **Embedded Postgres for local dev.** The DB package supports an embedded PostgreSQL mode so developers can run Paperclip without external database infrastructure.
- **Adapter abstraction.** AI agent execution is decoupled from the core via adapter packages (Claude, Codex, Cursor, Gemini, etc.), loaded at runtime.

## Boundaries

This domain covers source code architecture and implementation patterns. Deployment topology and CI/CD live in `../infrastructure`. Adapter implementations live in `../adapters`. Plugin SDK and extension contracts live in `../plugins`.
