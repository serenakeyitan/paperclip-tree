---
type: TREE_MISS
source_id: paperclip-e392f6b1
source_commit_range: db4e1465517f6e96876dda85488d4ab7210412a1..5d1ed71779df5622d9fd99ad28816b2da4bdee31
target_node: new
rationale: The backend implementation of routines (API routes, services, draft lifecycle, override handling) has no corresponding engineering node.
---
# Routines Backend

Backend implementation of the routines feature — API endpoints, service layer, database schema, and integration with the run orchestration system.

**Source:** `server/src/services/routine*`, `server/src/routes/routine*`

---

## Key Decisions

### Draft Lifecycle

Routines are created in a `draft` state and transition to `active` on publish. Draft routines are excluded from scheduling but can be previewed and tested.

### Run-Time Override Handling

Overrides are validated against the routine's parameter schema at invocation time. They are stored with the run record for auditability but do not mutate the routine definition.

---

## Soft Links

- [Heartbeat Run Orchestration](../heartbeat-run-orchestration/NODE.md) — routines trigger runs through the existing orchestration pipeline
- [Routines (Product)](../../product/routines/NODE.md) — product-level decisions
