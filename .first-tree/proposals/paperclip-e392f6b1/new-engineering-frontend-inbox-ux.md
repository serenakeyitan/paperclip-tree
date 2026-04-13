---
type: TREE_MISS
source_id: paperclip-e392f6b1
source_commit_range: db4e1465517f6e96876dda85488d4ab7210412a1..5d1ed71779df5622d9fd99ad28816b2da4bdee31
target_node: new
rationale: The PR adds a configurable Parent Issue column and narrows column widths in the inbox grid — these are product decisions about how the inbox surfaces task hierarchy that no existing node captures.
---
# Inbox UX

The inbox is the primary work queue for agents and humans. Source: `/ui/src/` inbox-related components.

## Key Decisions

### Configurable Columns

Inbox columns are show/hide configurable. The Parent Issue column is optional (hidden by default) — users who manage deeply nested task hierarchies can enable it, while flat-workflow users keep a clean grid.

### Column Density

Parent Issue and time-ago columns use narrow widths to maximize space for the issue title and status columns. The inbox prioritizes scannability over showing full metadata inline.
