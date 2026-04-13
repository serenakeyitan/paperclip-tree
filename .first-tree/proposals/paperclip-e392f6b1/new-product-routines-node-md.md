---
type: TREE_MISS
source_id: paperclip-e392f6b1
source_commit_range: db4e1465517f6e96876dda85488d4ab7210412a1..5d1ed71779df5622d9fd99ad28816b2da4bdee31
target_node: new
rationale: Routines (draft lifecycle, run-time overrides, trigger kinds, concurrency policies) are a new product-level concept with no dedicated tree node — existing references in engineering nodes are implementation mentions, not product decisions.
---
# Routines

Routines are scheduled or event-triggered automation definitions scoped to a company. They allow AI agents and human operators to define repeatable workflows that execute on a schedule (cron) or in response to events, with template variable interpolation for parameterization.

## Key Decisions

1. **Draft lifecycle** — Routines support a draft status, allowing definitions to be authored and reviewed before activation. A routine must be explicitly published before it will trigger.
2. **Run-time overrides** — When a routine is triggered, callers can supply override parameters that replace the routine's default variable bindings for that specific run. This enables reuse of a single routine definition across varying contexts.
3. **Concurrency policies** — Each routine declares a concurrency policy (e.g., skip, queue, cancel-previous) that governs what happens when a new trigger fires while a previous run is still in progress.
4. **Trigger kinds** — Routines support multiple trigger types (cron schedule, event-based, manual). The trigger kind is fixed at definition time.
5. **Company-scoped** — Like all Paperclip entities, routines are scoped to a `company_id` and included in the `.paperclip.yaml` sidecar for company portability.

## Relationships

Routines interact with the task system (routine runs may create tasks), governance (budget and approval gates apply to routine-initiated work), and the agent model (routines can target specific agents or roles).
