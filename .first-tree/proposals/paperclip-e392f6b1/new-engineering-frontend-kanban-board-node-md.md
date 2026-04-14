---
type: TREE_MISS
source_id: paperclip-e392f6b1
source_commit_range: db4e1465517f6e96876dda85488d4ab7210412a1..5d1ed71779df5622d9fd99ad28816b2da4bdee31
target_node: new
rationale: Collapsing empty kanban columns is a board view layout decision — no existing node covers kanban/board view behavior.
---
# Kanban Board

Layout and interaction decisions for the kanban board view — the drag-and-drop column-based view of issues grouped by status.

## Key Decisions

### Collapse Empty Columns

Empty kanban columns are collapsed to save horizontal space. This prevents the board from spreading across the full viewport when most status columns have no issues, keeping the populated columns visually prominent and reducing horizontal scrolling.
