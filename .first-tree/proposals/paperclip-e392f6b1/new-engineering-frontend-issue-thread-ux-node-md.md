---
type: TREE_SUPPLEMENT
source_id: paperclip-e392f6b1
source_commit_range: db4e1465517f6e96876dda85488d4ab7210412a1..5d1ed71779df5622d9fd99ad28816b2da4bdee31
target_node: new
rationale: The existing node covers rendering and routing but has no mention of performance constraints or optimizations for long threads — the typing lag fix introduces a performance-relevant decision about how long comment threads are handled.
---
### Performance in Long Threads

Long comment threads (many comments or large markdown payloads) can cause typing lag in the thread input. The thread view optimizes rendering to avoid re-rendering the full comment list on every keystroke, ensuring responsive input even in threads with hundreds of comments.

Source: PR #3163 (`PAPA-63`)
