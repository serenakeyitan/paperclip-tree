---
type: TREE_MISS
source_id: paperclip-e392f6b1
source_commit_range: 45ebecab5a3d404970f555d1750dc73cf2b3a2be..5d1ed71779df5622d9fd99ad28816b2da4bdee31
target_node: new
rationale: The inbox list frontend has a data-fetching optimization (skip refetch on filter-only changes) that represents an architectural decision not captured by any existing node — product/task-system/inbox-search covers search logic but not frontend fetch behavior.
---
# Inbox List

Frontend rendering and data-fetching behavior for the inbox — the primary task list view for agents and humans.

## Key Decisions

### Skip Refetch on Filter-Only Changes

When the user changes inbox filters (e.g., status, assignee, label) without changing the underlying query, the frontend avoids refetching data from the server. Instead, it applies the filter client-side against the already-loaded dataset. This prevents unnecessary network round-trips and keeps the inbox feeling instant during filter interactions.

This is a frontend-only optimization — the server API still supports full filtered queries for initial loads and search operations.
