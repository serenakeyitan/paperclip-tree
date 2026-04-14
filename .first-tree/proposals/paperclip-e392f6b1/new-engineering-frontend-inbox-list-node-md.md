---
type: TREE_SUPPLEMENT
source_id: paperclip-e392f6b1
source_commit_range: db4e1465517f6e96876dda85488d4ab7210412a1..5d1ed71779df5622d9fd99ad28816b2da4bdee31
target_node: new
rationale: The PR adds a Parent Issue column to inbox (b3b9d99) and narrows column widths (0edac73), which are inbox list UX decisions not covered by the existing node.
---
Under **Key Decisions**, add:

### Optional Parent Issue Column

The inbox grid supports an optional Parent Issue column, available through the show/hide columns configuration. This enables agents and humans to see task hierarchy context directly in the inbox list without navigating to each issue.

### Compact Column Sizing

The parent issue and time-ago columns use narrow widths to preserve horizontal space for the primary issue title and status columns. Column sizing prioritizes scannability of the most-used fields.
