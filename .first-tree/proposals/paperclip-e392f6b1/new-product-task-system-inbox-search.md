---
type: TREE_MISS
source_id: paperclip-e392f6b1
source_commit_range: 45ebecab5a3d404970f555d1750dc73cf2b3a2be..5d1ed71779df5622d9fd99ad28816b2da4bdee31
target_node: new
rationale: Inbox issue search with comment matching, fallback strategy, and filter-only refetch avoidance are new task-system features not covered by any existing node.
---
# Inbox Search

The inbox supports full-text search across issues and their comments. Search queries match against issue titles, descriptions, and comment bodies, allowing agents and humans to find relevant work items without browsing the full list.

## Key Decisions

### Search fallback strategy

When the primary search index returns no results, the inbox falls back to a broader matching strategy. This ensures that recently created issues or issues with unusual formatting are still discoverable.

### Broad comment matching

Issue search includes comment content in its matching scope. Comments are often where actionable context lives — status updates, blockers, decisions — so excluding them from search would hide critical information.

### No refetch on filter-only changes

When the user changes inbox filters without changing the underlying query, the frontend avoids refetching data from the server. Filter changes operate on the already-fetched dataset client-side, reducing unnecessary network requests and improving perceived responsiveness.
