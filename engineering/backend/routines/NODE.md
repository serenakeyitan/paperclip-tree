---
title: "Routines Backend"
owners: [cryppadotta]
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
