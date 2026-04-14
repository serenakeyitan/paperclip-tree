---
type: TREE_SUPPLEMENT
source_id: paperclip-e392f6b1
source_commit_range: db4e1465517f6e96876dda85488d4ab7210412a1..5d1ed71779df5622d9fd99ad28816b2da4bdee31
target_node: new
rationale: Commit 5136381 speeds up issue search — the existing inbox search node covers search behavior but not performance optimization decisions.
---
### Search Performance Optimization

Issue search has been optimized for speed to keep the inbox responsive during rapid agent triaging. Performance is a first-class concern for search, not just correctness.
