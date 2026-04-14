---
type: TREE_SUPPLEMENT
source_id: paperclip-e392f6b1
source_commit_range: db4e1465517f6e96876dda85488d4ab7210412a1..5d1ed71779df5622d9fd99ad28816b2da4bdee31
target_node: new
rationale: The PR enables agents to re-checkout tasks that are in_review when comment feedback arrives — a new checkout trigger not covered by the existing auto-checkout node which only describes wake-scoped checkout.
---
### Re-Checkout on Comment Feedback

When an agent receives comment feedback on a task that is in `in_review` status, the agent can re-checkout (transition back to `in_progress`) that task to address the feedback. This extends the auto-checkout model beyond initial wake events to cover the review-feedback loop.

**Rationale:** Review feedback is the most common reason an agent needs to resume work on a task it already completed. Without re-checkout, agents would need manual intervention to pick up feedback on their own reviewed work.
