---
type: TREE_SUPPLEMENT
source_id: paperclip-e392f6b1
source_commit_range: db4e1465517f6e96876dda85488d4ab7210412a1..5d1ed71779df5622d9fd99ad28816b2da4bdee31
target_node: new
rationale: The node documents session key strategies (issue/fixed/run) but does not mention that session keys are prefixed with the configured agent ID to prevent collisions when multiple agents share the same gateway.
---
Add to **Key Decisions** section:

- **Agent-ID-prefixed session keys.** Session keys are prefixed with the configured agent ID so that multiple Paperclip agents connecting to the same OpenClaw gateway do not collide on session routing. Without the prefix, two agents using the same session key strategy (e.g., both using `issue` mode on the same issue) would share a session unintentionally.
