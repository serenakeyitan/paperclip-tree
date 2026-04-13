---
type: TREE_MISS
source_id: paperclip-e392f6b1
source_commit_range: db4e1465517f6e96876dda85488d4ab7210412a1..5d1ed71779df5622d9fd99ad28816b2da4bdee31
target_node: new
rationale: Existing inbox-list node only covers filter refetch optimization — the PR adds configurable columns (Parent Issue column, show/hide, narrow widths) which is a new UX pattern not captured.
---
# Inbox Configurable Columns

The inbox list supports optional, user-configurable columns beyond the default set.

## Key Decisions

### Optional Parent Issue Column

A Parent Issue column can be toggled on in the inbox grid, surfacing the task hierarchy directly in the list view. This helps agents and humans understand issue context without opening each issue.

### Narrow Auxiliary Columns

Auxiliary columns (Parent Issue, time-ago) use narrow widths to preserve horizontal space for issue titles, which are the primary scanning target in the inbox.
