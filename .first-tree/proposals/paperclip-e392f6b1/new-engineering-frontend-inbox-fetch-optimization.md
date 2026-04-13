---
type: TREE_MISS
source_id: paperclip-e392f6b1
source_commit_range: 45ebecab5a3d404970f555d1750dc73cf2b3a2be..5d1ed71779df5622d9fd99ad28816b2da4bdee31
target_node: new
rationale: The inbox refetch optimization (avoid refetching on filter-only changes) is a frontend data-fetching convention not captured by the existing inbox-search product node or any frontend node.
---
# Inbox Fetch Optimization

Data-fetching conventions for the inbox view — how the frontend avoids unnecessary API calls when interacting with inbox filters and search.

## Key Decisions

### Skip Refetch on Filter-Only Changes

When a user changes inbox filters (e.g., status, assignee, priority) without changing the underlying data query, the frontend skips refetching from the server. Filter-only changes are applied client-side against the already-fetched dataset. This prevents redundant network requests and keeps the inbox responsive during rapid filter toggling.

This convention applies specifically to the inbox list view. Other views that share filter components may still refetch if their data model requires it.

### Distinction from Search

Search queries (covered in the product task-system inbox-search node) always trigger a server-side fetch because they change the result set. Filter changes narrow an existing result set and do not require a round-trip. This distinction is enforced in the inbox query hook layer.
