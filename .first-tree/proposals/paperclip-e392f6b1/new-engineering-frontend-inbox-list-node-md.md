---
type: TREE_SUPPLEMENT
source_id: paperclip-e392f6b1
source_commit_range: db4e1465517f6e96876dda85488d4ab7210412a1..5d1ed71779df5622d9fd99ad28816b2da4bdee31
target_node: new
rationale: Commits b3b9d99 and 0edac73 add a configurable Parent Issue column and narrow column widths in the inbox grid — the existing inbox list node doesn't cover column customization.
---
### Configurable Column Visibility

The inbox grid supports show/hide toggling for optional columns, including a Parent Issue column. This lets agents and humans tailor the inbox view to their workflow without changing the underlying data fetch.

### Compact Column Sizing

Metadata columns (parent issue, time-ago) use narrow widths to maximize space for the primary issue title and status columns.
