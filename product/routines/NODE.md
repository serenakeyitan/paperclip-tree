---
title: "Routines"
owners: [cryppadotta]
---

# Routines

Scheduled or event-triggered automation definitions scoped to a company. Routines allow AI agents and human operators to define repeatable workflows that execute on a schedule (cron) or in response to events, with template variable interpolation for parameterization.

**Source:** `server/src/services/routine*`, `server/src/routes/routine*`, `shared/src/routine-variables.ts`, `cli/src/commands/routines.ts`

---

## Key Decisions

### Draft Lifecycle
Routines support a draft status, allowing definitions to be authored and reviewed before activation. A routine must be explicitly published before it will trigger. This prevents half-configured automations from firing unexpectedly.

### Run-Time Overrides
When a routine is triggered, callers can supply override parameters that replace the routine's default variable bindings for that specific run. This enables reuse of a single routine definition across varying contexts without duplicating definitions.

### Concurrency Policies
Each routine declares a concurrency policy (e.g., skip, queue, cancel-previous) that governs what happens when a new trigger fires while a previous run is still in progress.

### Trigger Kinds
Routines support multiple trigger types: cron schedule, event-based, and manual. The trigger kind is fixed at definition time.

### Company-Scoped
Like all Paperclip entities, routines are scoped to a `company_id` and included in the `.paperclip.yaml` sidecar for company portability.

---

## Relationships

- **Task system** — Routine runs may create tasks.
- **Governance** — Budget and approval gates apply to routine-initiated work.
- **Agent model** — Routines can target specific agents or roles.
- **Database** — `routines` table stores definitions; run history tracked separately.
- **CLI** — `routines` command group for managing routine definitions.
- **Frontend** — Routines management UI for creating, editing, and monitoring routines.
