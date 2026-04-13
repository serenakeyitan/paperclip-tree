---
type: TREE_MISS
source_id: paperclip-e392f6b1
source_commit_range: db4e1465517f6e96876dda85488d4ab7210412a1..5d1ed71779df5622d9fd99ad28816b2da4bdee31
target_node: new
rationale: The task system node defines inbox conceptually ('tasks assigned + comments') but has no node capturing inbox UX workflow decisions — how work items are surfaced, filtered, sorted, and acted upon as an actionable workflow surface.
---
# Inbox Workflows

The inbox is the primary entry point for agents and humans to see what needs their attention. It aggregates tasks assigned to them, comments on tasks they're involved in, and status changes on watched items.

## Key Decisions

### Inbox as a Workflow Surface

The inbox is not just a notification list — it is an actionable workflow surface. Users and agents can triage, filter, and act on items directly from the inbox without navigating to individual issue detail views. This reduces context-switching for agents processing multiple tasks.

**Rationale:** Agents operating autonomously need to efficiently process their work queue. A passive notification list forces round-trips to detail views for every action. An actionable inbox keeps the agent's decision loop tight.

### Issue List Integration

The issue list and inbox share filtering and display patterns but serve different purposes: the issue list is a team-scoped view of all work, while the inbox is a personal view of items requiring the viewer's attention. Their interaction patterns are aligned so users build one mental model for navigating work items.

## Open Questions

- Inbox prioritization logic — how items are ranked (recency vs. urgency vs. blocking status)
- Read/unread state tracking for inbox items
- Agent-specific inbox behaviors vs. human inbox behaviors
