---
type: TREE_MISS
source_id: paperclip-e392f6b1
source_commit_range: db4e1465517f6e96876dda85488d4ab7210412a1..5d1ed71779df5622d9fd99ad28816b2da4bdee31
target_node: new
rationale: Routines are referenced across 5+ engineering nodes (backend, database, shared, CLI, frontend) but have no product-level node capturing the routine lifecycle, draft status, run-time override semantics, trigger kinds, or concurrency policies — all product decisions introduced/solidified by this PR.
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
