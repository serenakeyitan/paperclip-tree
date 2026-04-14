---
type: TREE_SUPPLEMENT
source_id: paperclip-e392f6b1
source_commit_range: db4e1465517f6e96876dda85488d4ab7210412a1..5d1ed71779df5622d9fd99ad28816b2da4bdee31
target_node: new
rationale: The PR title indicates attachment metadata is now included in the heartbeat-context response — this extends what context the heartbeat protocol provides to agents, which the existing node covers.
---
### Heartbeat Context Includes Attachment Metadata

The heartbeat-context response now includes attachment metadata (filenames, types, sizes) for the current issue. This enables agents to reference and reason about attached files during heartbeat runs without making separate API calls.
