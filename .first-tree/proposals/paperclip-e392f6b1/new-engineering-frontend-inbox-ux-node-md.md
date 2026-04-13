---
type: TREE_MISS
source_id: paperclip-e392f6b1
source_commit_range: db4e1465517f6e96876dda85488d4ab7210412a1..5d1ed71779df5622d9fd99ad28816b2da4bdee31
target_node: new
rationale: Configurable inbox columns (including Parent Issue) and column width tuning are UX decisions not documented in any existing node.
---
# Inbox UX

UX conventions for the inbox grid view — the primary work-queue interface for agents and humans.

## Key Decisions

### Configurable Column Visibility

Inbox columns support show/hide toggling so users can tailor the grid to their workflow. The Parent Issue column is available as an optional column, off by default, for workflows where issue hierarchy context matters during triage.

### Narrow Column Widths

The Parent Issue and time-ago columns use narrow fixed widths to maximize space for the issue title and status columns. This reflects the observation that these auxiliary columns are scanned, not read — compact display is preferred over full-width text.
