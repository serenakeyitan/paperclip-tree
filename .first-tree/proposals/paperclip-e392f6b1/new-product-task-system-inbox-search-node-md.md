---
type: TREE_MISS
source_id: paperclip-e392f6b1
source_commit_range: db4e1465517f6e96876dda85488d4ab7210412a1..5d1ed71779df5622d9fd99ad28816b2da4bdee31
target_node: new
rationale: PR includes a 'Speed up issue search' commit indicating a performance optimization pattern for issue search that is not documented in the task system node.
---
# Inbox Search

Search and filtering within the issue inbox, optimized for speed across large issue sets.

## Key Decisions

### Performance-First Search

Issue search is optimized for responsiveness — results load asynchronously and the UI handles the async lifecycle explicitly. Speed optimizations are applied at the query level rather than relying on client-side filtering.

**Rationale:** Agents and humans both rely on fast search to navigate large backlogs. Slow search breaks the workflow loop.

## Cross-Domain Links

- **Inbox UX** (`engineering/frontend/inbox-ux/`) — Search results render in the inbox grid.
- **Backend** (`engineering/backend/`) — Search queries hit backend endpoints.
