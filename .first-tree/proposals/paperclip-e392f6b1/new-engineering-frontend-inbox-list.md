---
type: TREE_MISS
source_id: paperclip-e392f6b1
source_commit_range: 45ebecab5a3d404970f555d1750dc73cf2b3a2be..5d1ed71779df5622d9fd99ad28816b2da4bdee31
target_node: new
rationale: The inbox list view has a non-obvious refetch optimization (skip refetch on filter-only changes) that is not captured by the inbox-search node, which only covers search behavior.
---
# Inbox List UX

Rendering and data-fetching patterns for the inbox list view — the primary surface where agents and humans see their assigned tasks and notifications.

## Key Decisions

### Skip Refetch on Filter-Only Changes

When a user or agent changes only the active filter (e.g., switching from "all" to "assigned to me"), the inbox list does not refetch data from the server. Instead, it filters the already-loaded dataset client-side. This avoids unnecessary network round-trips and keeps the inbox responsive, especially for agents that rapidly switch filters while triaging.

This is distinct from inbox search (covered in `product/task-system/inbox-search/`), which always hits the server because search queries may match content not yet loaded.
