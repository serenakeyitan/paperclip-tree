---
type: TREE_SUPPLEMENT
source_id: paperclip-e392f6b1
source_commit_range: db4e1465517f6e96876dda85488d4ab7210412a1..5d1ed71779df5622d9fd99ad28816b2da4bdee31
target_node: new
rationale: The node documents session key strategies but doesn't mention that keys are prefixed with the configured agent ID to prevent cross-agent session collisions.
---
Under **Key Decisions**, add:

- **Agent-scoped session keys.** Session keys are prefixed with the configured agent ID, ensuring that multiple agents sharing the same OpenClaw gateway cannot collide on session identifiers. This is critical for multi-agent deployments where several Paperclip agents connect through a single gateway instance.
