---
type: TREE_MISS
source_id: paperclip-e392f6b1
source_commit_range: a3e125f79659e9d6a2caac8ff3a0eb3cd4127039..d6b06788f6efacb002791c1a60b4889d7bfdb22d
target_node: new
rationale: Routines are referenced across 5 engineering nodes (backend, database, shared, CLI, company-model) but have no product-level node capturing what routines are, their lifecycle (draft → active), run-time override semantics, trigger kinds, or concurrency policies — all of which are product decisions, not implementation details.
---
## Overview

Routines are scheduled or event-triggered automation definitions scoped to a company. They allow AI agents and human operators to define repeatable workflows that execute on a schedule (cron) or in response to events, with template variable interpolation for parameterization.

## Key Decisions

1. **Draft lifecycle** — Routines support a draft status, allowing definitions to be authored and reviewed before activation. A routine must be explicitly published before it will trigger.
2. **Run-time overrides** — When a routine is triggered, callers can supply override parameters that replace the routine's default variable bindings for that specific run. This enables reuse of a single routine definition across varying contexts.
3. **Concurrency policies** — Each routine declares a concurrency policy (e.g., skip, queue, cancel-previous) that governs what happens when a new trigger fires while a previous run is still in progress.
4. **Trigger kinds** — Routines support multiple trigger types (cron schedule, event-based, manual). The trigger kind is fixed at definition time.
5. **Company-scoped** — Like all Paperclip entities, routines are scoped to a `company_id` and included in the `.paperclip.yaml` sidecar for company portability.

## Relationships

Routines interact with the task system (routine runs may create tasks), governance (budget and approval gates apply to routine-initiated work), and the agent model (routines can target specific agents or roles).
