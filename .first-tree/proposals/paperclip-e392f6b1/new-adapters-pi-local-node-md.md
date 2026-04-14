---
type: TREE_SUPPLEMENT
source_id: paperclip-e392f6b1
source_commit_range: db4e1465517f6e96876dda85488d4ab7210412a1..5d1ed71779df5622d9fd99ad28816b2da4bdee31
target_node: new
rationale: The PR title 'Treat Pi quota exhaustion as a failed run' adds quota-exhaustion error handling to the Pi adapter — the existing node likely doesn't cover this behavior.
---
### Quota Exhaustion Treated as Failed Run

When the Pi adapter encounters a quota exhaustion response, it marks the current run as failed rather than retrying or hanging. This prevents agents from spinning on an unrecoverable error and ensures the heartbeat orchestration layer can reassign or escalate the work.
