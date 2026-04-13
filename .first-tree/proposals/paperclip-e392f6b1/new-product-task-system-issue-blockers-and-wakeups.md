---
type: TREE_MISS
source_id: paperclip-e392f6b1
source_commit_range: db4e1465517f6e96876dda85488d4ab7210412a1..5d1ed71779df5622d9fd99ad28816b2da4bdee31
target_node: new
rationale: The PR ships first-class blocking relationships with automatic dependency wakeup events — this was explicitly an open question in product/task-system/NODE.md and drift/.../issue-links/NODE.md, and is now a concrete shipped feature with significant agent coordination implications.
---
# Issue Blockers & Dependency Wakeups

Issues can declare directed blocking/blocked-by relationships. When a blocking issue completes, the system automatically emits wakeup events to all dependent issues, enabling assigned agents to resume work without polling or manual intervention.

## Key Decisions

### Blocking as a First-Class Relationship

Blocking/blocked-by is the primary link type with enforced semantics: a blocked issue cannot proceed until all its blockers are resolved. Blockers are visible in the issue sidebar and drive agent coordination through wakeup events.

**Rationale:** Agents working autonomously need a reliable, event-driven signal that their dependencies are met. Without first-class blocking semantics, agents would need to poll or rely on human coordination to know when to resume.

### Automatic Dependency Wakeups

When a blocking issue transitions to a terminal state (`done` or `cancelled`), the system emits wakeup events to all issues that were blocked by it. This triggers assigned agents to re-evaluate and potentially resume work.

**Rationale:** Polling for dependency resolution wastes cycles and introduces latency. Wakeup events make the dependency graph an active coordination mechanism rather than a passive data structure.

### Sub-Issues Default to Parent Workspace

Sub-issues automatically inherit their parent issue's workspace assignment, reducing friction when decomposing work.

## Boundaries

This node covers the link semantics, blocking model, and wakeup mechanism. The task system hierarchy and status workflow are documented in the parent [task-system](../NODE.md) node.

## Open Questions

- Additional link types beyond blocking (related, duplicate) and their semantics
- Whether circular blocking dependencies are detected and prevented at creation time
- Wakeup behavior differences when a blocker is cancelled vs. completed
