---
type: TREE_SUPPLEMENT
source_id: paperclip-e392f6b1
source_commit_range: 45ebecab5a3d404970f555d1750dc73cf2b3a2be..5d1ed71779df5622d9fd99ad28816b2da4bdee31
target_node: new
rationale: The PR changes the checkout mechanism from agent-initiated API calls to harness-automated checkout, which strengthens the 'Harness-Level Automation' decision already documented but may add nuance about linked worktree reuse during auto-checkout.
---
### Linked Worktree Reuse on Auto-Checkout

When auto-checkout triggers for a scoped wake, the harness reuses any existing linked worktree for that issue rather than creating a new one. This preserves agent work-in-progress across wake cycles and ties the auto-checkout flow to the execution workspace lifecycle documented in [Execution Workspaces](../../../engineering/execution-workspaces/NODE.md).
