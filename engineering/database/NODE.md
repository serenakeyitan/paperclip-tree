---
title: "Database"
owners: [cryppadotta, serenakeyitan]
soft_links: ["engineering/backend", "engineering/shared", "engineering/cli/NODE.md", "plugins/runtime/NODE.md"]
---

# Database

PostgreSQL database layer using Drizzle ORM. Source: `/packages/db`.

Package: `@paperclipai/db`

## Stack

- **Drizzle ORM** with `postgres.js` driver
- **Drizzle Kit** for schema diffing and migration generation
- **PostgreSQL** (external or embedded)

## Schema Design

Schema files live in `/packages/db/src/schema/`, one file per domain entity. Key tables:

### Core Domain
- **companies** — the top-level organizational unit (an "AI company")
- **agents** — AI agents belonging to a company, with config revisions and runtime state
- **projects** — grouping of work within a company
- **issues** — tasks/tickets with status, priority, labels, comments, attachments, and execution decisions
- **goals** — OKR-style objectives linked to projects
- **routines** — scheduled or event-triggered automation definitions

### Governance & Collaboration
- **approvals / issue_approvals** — human-in-the-loop approval gates
- **budget_policies / budget_incidents** — spend control and incident tracking
- **company_memberships / invites / join_requests** — access control
- **principal_permission_grants** — fine-grained authorization

### Execution Infrastructure
- **execution_workspaces / workspace_operations / workspace_runtime_services** — sandboxed agent work environments
- **agent_task_sessions / agent_wakeup_requests** — agent execution lifecycle
- **heartbeat_runs / heartbeat_run_events** — periodic agent health checks

### Plugin System
- **plugins / plugin_config / plugin_state / plugin_jobs / plugin_logs** — plugin runtime persistence
- **plugin_entities / plugin_webhooks / plugin_company_settings** — plugin extension points

### Auth (BetterAuth)
- **auth_users / auth_sessions / auth_accounts / auth_verifications** — managed by BetterAuth, schema defined in Drizzle for co-location

### Operational
- **activity_log** — audit trail
- **cost_events / finance_events** — financial tracking
- **assets** — uploaded file metadata
- **feedback_exports / feedback_votes** — user feedback

## Multi-Company Isolation

All company-scoped tables include a `companyId` foreign key. Isolation is enforced at the query level in services — there is no row-level security or separate database per company. This is a deliberate simplicity trade-off: one database, one schema, query-scoped isolation.

## Migrations

- Migrations are in `/packages/db/src/migrations/`, numbered sequentially (0000, 0001, ...).
- Generated via `drizzle-kit generate` from the TypeScript schema.
- Applied at server startup via `drizzle-orm/postgres-js/migrator`.
- The DB package tracks migration state and supports inspecting pending migrations before applying.
- A migration numbering check script ensures sequential ordering.

## Embedded Postgres

For local development and single-user deployments, the DB package supports starting an embedded PostgreSQL process. This eliminates external database dependencies for getting started. The embedded mode stores data in `~/.paperclip/postgres/`.

## Key Decisions

- **Schema-as-code in Drizzle.** No raw SQL schema files — the TypeScript schema is the source of truth, and Drizzle Kit generates migrations from diffs.
- **Single database, query-level isolation.** Multi-tenancy is simple and avoids operational complexity of per-tenant databases.
- **Backup/restore built in.** The DB package includes `pg_dump`/`pg_restore` wrappers for data portability.
- **No ORM abstraction layer.** Services use Drizzle's query builder directly — no repository pattern wrapping it.

## Decision Records

- [company-scoped-isolation.md](company-scoped-isolation.md) — Why Paperclip keeps multi-company data in one schema while treating `company_id` as a hard isolation boundary across schema, auth, and service logic.
