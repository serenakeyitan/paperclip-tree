---
type: TREE_MISS
source_id: paperclip-e392f6b1
source_commit_range: a3e125f79659e9d6a2caac8ff3a0eb3cd4127039..d6b06788f6efacb002791c1a60b4889d7bfdb22d
target_node: new
rationale: The PR adds configurable inbox columns (Parent Issue, time-ago), parent-child nesting, and show/hide column controls — the tree only mentions inbox as a page bullet with no node documenting inbox UX patterns or design decisions.
---
# Inbox UX

The inbox is the primary entry point for agents and humans to see what needs their attention. It aggregates assigned tasks, mentions, and status changes into a unified feed.

## Key Decisions

### Inbox Is a Query, Not a Queue

There is no separate inbox table or notification store. The inbox view is computed from task assignments, comment mentions, and status transitions. This keeps the task system as the single source of truth.

### Configurable Columns

Users can show/hide columns in the inbox grid, including an optional Parent Issue column for navigating the task hierarchy. Column widths are tuned for scan speed (narrower parent issue and time-ago columns).

### Parent-Child Nesting

Sub-issues can be nested under their parent in the inbox view, giving hierarchy context without leaving the inbox.

## Boundaries

This node covers the inbox frontend UX. The underlying task and comment data model is in [task-system](../../product/task-system/NODE.md). The inbox is a view, not a separate data model.

## Open Questions

- Read/unread state — whether the inbox tracks what has been seen
- Inbox filtering and saved views
- Agent inbox API — whether agents consume the inbox through the same endpoint as the UI or a dedicated feed
