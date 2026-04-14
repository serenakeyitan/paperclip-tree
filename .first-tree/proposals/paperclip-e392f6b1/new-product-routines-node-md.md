---
type: TREE_MISS
source_id: paperclip-e392f6b1
source_commit_range: db4e1465517f6e96876dda85488d4ab7210412a1..5d1ed71779df5622d9fd99ad28816b2da4bdee31
target_node: new
rationale: No existing node covers 'routines' as a product concept — this is a new feature domain for defining repeatable agent workflows with draft states and run-time overrides.
---
# Routines

Routines define repeatable, scheduled agent workflows within a company. They allow operators to configure recurring tasks that agents execute on a cadence.

**Source:** `server/src/services/routine*`, `server/src/routes/routine*`, related UI components.

---

## Key Decisions

### Draft Routines

Routines support a draft state, allowing operators to configure and iterate on a routine before activating it. Draft routines are visible in the UI but do not trigger execution until explicitly published.

### Run-Time Overrides

When triggering a routine (manually or on schedule), callers can supply run-time overrides that modify parameters for that specific execution without changing the routine's saved configuration. This enables one-off customization of recurring workflows.

---

## Open Questions

- How do routines interact with governance approval gates?
- What is the relationship between routines and the heartbeat run orchestration lifecycle?
- Can plugins define or extend routines?
