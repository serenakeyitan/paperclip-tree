---
type: TREE_MISS
source_id: paperclip-e392f6b1
source_commit_range: db4e1465517f6e96876dda85488d4ab7210412a1..5d1ed71779df5622d9fd99ad28816b2da4bdee31
target_node: new
rationale: The PR speeds up issue search with async rendering — search behavior and performance strategy are not covered by the existing task-system node.
---
# Issue Search

How issue search works across the inbox and issue views.

## Key Decisions

### Relevance-Ranked Multi-Field Search

Issue search matches across multiple fields (title, description, comments) and ranks results by relevance rather than simple recency. This is critical for agents that need to find related work quickly — substring matching was too noisy in companies with thousands of issues.

### Performance-First Design

Search is optimized for speed at the query layer. The UI waits for async results and renders progressively, keeping the inbox responsive even during complex searches. This was a deliberate choice over pre-computed search indices, which would add operational complexity.
