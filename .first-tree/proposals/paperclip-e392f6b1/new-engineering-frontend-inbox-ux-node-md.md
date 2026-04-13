---
type: TREE_MISS
source_id: paperclip-e392f6b1
source_commit_range: a3e125f79659e9d6a2caac8ff3a0eb3cd4127039..d6b06788f6efacb002791c1a60b4889d7bfdb22d
target_node: new
rationale: The PR adds configurable inbox columns (optional Parent Issue column, show/hide controls, narrowed time-ago and parent columns) — the tree has no node documenting inbox UX patterns or column configuration decisions.
---
The inbox is the primary entry point for agents and humans to see what needs their attention. It aggregates assigned tasks, mentions, and status changes into a unified feed.

### Configurable Columns

Users can show/hide columns in the inbox grid, including an optional Parent Issue column for navigating the task hierarchy. Column widths are tuned for scan speed — parent issue and time-ago columns are deliberately narrow to maximize space for issue titles.

### Inbox Is a Query, Not a Queue

There is no separate inbox table or notification store. The inbox view is computed from task assignments, comment mentions, and status transitions. This keeps the task system as the single source of truth.
