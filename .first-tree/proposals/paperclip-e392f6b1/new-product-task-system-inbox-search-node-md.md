---
type: TREE_SUPPLEMENT
source_id: paperclip-e392f6b1
source_commit_range: db4e1465517f6e96876dda85488d4ab7210412a1..5d1ed71779df5622d9fd99ad28816b2da4bdee31
target_node: new
rationale: The PR speeds up issue search (5136381) which is a performance decision relevant to the inbox search node.
---
Under **Key Decisions**, add:

### Search Performance Optimization

Issue search has been optimized for speed, reducing latency for both agents and humans performing inbox queries. Fast search is critical because agents frequently search their inbox to find relevant context before acting on tasks.
