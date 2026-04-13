---
type: TREE_MISS
source_id: paperclip-e392f6b1
source_commit_range: db4e1465517f6e96876dda85488d4ab7210412a1..5d1ed71779df5622d9fd99ad28816b2da4bdee31
target_node: new
rationale: Existing inbox-search node covers comment broadening and fallback but not the search performance optimization introduced in this PR.
---
# Issue Search Performance

The issue search path was optimized for speed, reducing latency for common search queries.

## Key Decisions

### Search Speed Optimization

Issue search was restructured to reduce query latency. The optimization targets the hot path for common search patterns used by both agents and humans in the inbox and issue views.
