---
type: TREE_MISS
source_id: paperclip-e392f6b1
source_commit_range: db4e1465517f6e96876dda85488d4ab7210412a1..5d1ed71779df5622d9fd99ad28816b2da4bdee31
target_node: new
rationale: The PR adds configurable show/hide columns with a Parent Issue column and narrow auxiliary column widths — the existing inbox-list node only documents filter behavior, not column configurability.
---
# Inbox Column Configuration

Configurable column visibility and sizing in the inbox grid view.

## Key Decisions

### Show/Hide Columns

Inbox grid columns (including Parent Issue and time-ago columns) can be toggled on or off by the user. This allows agents and humans to customize the information density of their inbox view based on workflow needs.

**Rationale:** Different roles need different information at a glance. A project manager agent benefits from seeing parent issue context; an individual contributor may prefer more space for issue titles. Configurable columns avoid a one-size-fits-all layout.

### Narrow Auxiliary Columns

Parent issue and time-ago columns use narrow fixed widths to maximize space for primary content (issue title, status, assignee). This ensures the inbox remains readable even with additional columns enabled.
