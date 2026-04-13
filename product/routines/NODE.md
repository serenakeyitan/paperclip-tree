---
title: "Routines"
owners: [bingran-you, cryppadotta, serenakeyitan]
---

# Routines

Routines are scheduled or event-triggered automation definitions scoped to a company. They allow AI agents and human operators to define repeatable workflows that execute on a schedule (cron) or in response to events, with template variable interpolation for parameterization.

**Source:** `server/src/services/routine*`, `server/src/routes/routine*`, `shared/src/constants/routine*`

---

## Key Decisions

### Draft Lifecycle
Routines support a draft status, allowing definitions to be authored and reviewed before activation. A routine must be explicitly published before it will trigger. This prevents accidental execution of incomplete or unreviewed automation.

### Run-time Overrides
When a routine is triggered, callers can supply override parameters that replace the routine's default variable bindings for that specific run. This enables reuse of a single routine definition across varying contexts without modifying the base definition.

### Concurrency Policies
Each routine declares a concurrency policy (e.g., skip, queue, cancel-previous) that governs what happens when a new trigger fires while a previous run is still in progress. This prevents runaway parallel execution and gives operators fine-grained control over overlap behavior.

### Trigger Kinds
Routines support multiple trigger types: cron schedule, event-based, and manual. The trigger kind is fixed at definition time.

### Company-Scoped
Like all Paperclip entities, routines are scoped to a `company_id` and included in the `.paperclip.yaml` sidecar for company portability.

## Relationships

Routines interact with the **task system** (routine runs may create tasks), **governance** (budget and approval gates apply to routine-initiated work), and the **agent model** (routines can target specific agents or roles).
