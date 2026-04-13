---
type: TREE_MISS
source_id: paperclip-e392f6b1
source_commit_range: db4e1465517f6e96876dda85488d4ab7210412a1..5d1ed71779df5622d9fd99ad28816b2da4bdee31
target_node: new
rationale: The PR includes a 'Speed up issue search' commit with dedicated test coverage — search performance and matching strategy are not documented in any tree node.
---
# Issue Search

Server-side issue search powering the inbox and issue picker. Optimized for speed and relevance in repositories with large issue counts.

## Key Decisions

### Performance-First Search

Issue search is optimized for low-latency results to support real-time filtering in the inbox and issue selection UIs. Search results are returned asynchronously and the UI waits for results before rendering.

**Rationale:** Agents and humans both need fast issue lookup — agents to find related work and dependencies, humans to navigate their inbox. Slow search creates friction in both workflows.
