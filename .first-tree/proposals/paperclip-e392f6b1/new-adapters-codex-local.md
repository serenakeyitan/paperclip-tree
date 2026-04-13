---
type: TREE_MISS
source_id: paperclip-e392f6b1
source_commit_range: db4e1465517f6e96876dda85488d4ab7210412a1..5d1ed71779df5622d9fd99ad28816b2da4bdee31
target_node: new
rationale: PR #3383 adds fast mode support to the Codex Local adapter, but the existing node documents no fast mode capability or decision.
---
Update the existing Capabilities section to add:

- **Fast mode:** Supported. Allows Paperclip to invoke Codex in fast mode for lower-latency responses at the cost of reduced reasoning depth. Controlled via adapter configuration.

Update the Key Decisions section to add:

- **Fast mode as an adapter-level concern** — Fast mode is toggled at the adapter layer rather than the task layer, keeping the task system runtime-agnostic while letting operators choose speed vs. depth per adapter instance.
