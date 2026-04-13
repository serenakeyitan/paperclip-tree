---
type: TREE_MISS
source_id: paperclip-e392f6b1
source_commit_range: 45ebecab5a3d404970f555d1750dc73cf2b3a2be..5d1ed71779df5622d9fd99ad28816b2da4bdee31
target_node: new
rationale: The promoted tree has no node for issue-to-issue links; a drift node exists but lacks the blocking-as-first-class semantics and automatic dependency wakeup coordination model introduced by this PR.
---
# Issue Links & Dependency Wakeups

Issues can be linked to other issues to express directed relationships — primarily **blocking** and **blocked-by**. These links are the backbone of agent coordination: when a blocking issue resolves, dependent issues are automatically woken up, enabling autonomous agents to resume work without polling or human intervention.

## Key Decisions

### Direct Link Navigation Optimized for Speed

Issue-to-issue links are optimized for fast traversal. When an agent or user views an issue, linked issues are immediately navigable without additional lookups. This was a performance-critical decision — agents frequently need to follow dependency chains across many linked issues, and slow traversal creates bottlenecks in autonomous decision-making.

### Blocking as a First-Class Relationship

Blocking/blocked-by is the primary link type with enforced semantics: a blocked issue cannot proceed until all its blockers are resolved. This is surfaced in the UI (blocker fields in the issue sidebar) and in agent coordination (wakeup events on blocker completion).

**Rationale:** Agents working autonomously need a reliable, structured signal that dependencies are met. Without first-class blocking semantics, agents would need to poll or rely on human coordination.

### Automatic Dependency Wakeups

When a blocking issue transitions to a terminal state (`done` or `cancelled`), the system emits wakeup events to all blocked issues. This triggers assigned agents to re-evaluate and potentially resume work, making the dependency graph an active coordination mechanism rather than passive metadata.

## Boundaries

This node covers link semantics, the blocking model, navigation performance, and the wakeup mechanism. Task hierarchy and status workflows are in the parent [task-system](../NODE.md) node. Frontend rendering of blockers is documented in [issue-thread-ux](../issue-thread-ux/NODE.md).

## Open Questions

- Additional link types beyond blocking (related, duplicate) and their semantics
- Whether circular blocking dependencies are detected and prevented at creation time
- Wakeup behavior differences when a blocker is cancelled vs. completed
