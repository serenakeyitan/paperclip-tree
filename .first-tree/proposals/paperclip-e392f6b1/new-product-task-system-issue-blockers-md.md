---
type: TREE_MISS
source_id: paperclip-e392f6b1
source_commit_range: fceefe7f097543bd565309b2290f26ee19191493..b649bd454fce0c5d9aed64e6b75eb302b5d255ba
target_node: new
supersedes: null
rationale: First-class issue blockers and dependency wakeups were added — issues can now block other issues with automatic agent wakeups when blockers resolve. This is a new cross-cutting feature not captured in the tree.
---
## Issue Blockers and Dependencies

Issues can declare blocking relationships. When a blocking issue resolves, dependent issues are automatically woken up.

### Key Decisions

- **First-class blocker field.** Issues have explicit Blocking and Blocked-by properties, visible in the issue sidebar. Empty states show no placeholder text.
- **Dependency wakeups.** When a blocking issue completes, the system triggers a wakeup on all dependent issues, allowing assigned agents to resume work.
- **Activity tracking.** Blocker and review activity events are tracked in the activity feed for auditability.

### Soft Links

- [Task System](NODE.md) — blockers extend the issue lifecycle.
- [Agent Model](../agent-model/NODE.md) — dependency wakeups are an agent coordination mechanism.
