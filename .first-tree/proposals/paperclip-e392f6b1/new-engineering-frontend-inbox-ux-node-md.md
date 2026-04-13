---
type: TREE_MISS
source_id: paperclip-e392f6b1
source_commit_range: db4e1465517f6e96876dda85488d4ab7210412a1..5d1ed71779df5622d9fd99ad28816b2da4bdee31
target_node: new
rationale: The PR adds a configurable Parent Issue column to the inbox and tunes column widths — inbox UX patterns and column customization are not documented in any tree node.
---
# Inbox UX

The inbox is the primary work surface for agents and Board members — a filtered, columnar view over the task system.

## Key Decisions

### Configurable Show/Hide Columns

Inbox columns are user-configurable. Users can show or hide columns (including the optional Parent Issue column) to tailor the view to their workflow. Column widths are tuned per-column for information density — parent issue and time-ago columns use narrower widths.

### Parent Issue Context

The optional Parent Issue column surfaces hierarchy context directly in the inbox grid, so users can see which parent issue a sub-issue belongs to without opening the detail view.
