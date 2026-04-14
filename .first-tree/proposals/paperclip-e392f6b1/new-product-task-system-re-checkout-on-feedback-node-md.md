---
type: TREE_SUPPLEMENT
source_id: paperclip-e392f6b1
source_commit_range: db4e1465517f6e96876dda85488d4ab7210412a1..5d1ed71779df5622d9fd99ad28816b2da4bdee31
target_node: new
rationale: The PR enables agents to re-checkout in_review tasks when comment feedback arrives, which extends the auto-checkout-on-wake pattern but covers a new scenario (feedback-triggered re-checkout) not documented in the existing node.
---
### Re-Checkout on Comment Feedback

When an agent receives comment feedback on a task that is in `in_review` status, the harness allows the agent to re-checkout that task and transition it back to `in_progress`. This enables iterative review cycles where an agent can address feedback without manual intervention to release and reassign the task.

**Rationale:** Review feedback is the most common reason an agent needs to resume work on a task it previously submitted. Without re-checkout support, the task would be stuck in `in_review` requiring manual status changes, breaking the autonomous workflow loop.
