---
type: TREE_SUPPLEMENT
source_id: paperclip-e392f6b1
source_commit_range: 45ebecab5a3d404970f555d1750dc73cf2b3a2be..5d1ed71779df5622d9fd99ad28816b2da4bdee31
target_node: new
rationale: The PR improves scoped wake checkout and linked worktree reuse — both already documented in this node — but the specific improvement (moving checkout from agent-initiated API call to harness-automated) may refine the 'Scoped Wake Checkout' section's description of how checkout is triggered.
---
### Scoped Wake Checkout

Update to clarify that scoped wake checkout is now fully harness-driven rather than requiring agents to call the checkout API themselves on boot. The harness intercepts the wake event and performs the checkout automatically before the agent session begins, removing adapter-level checkout logic and ensuring consistent behavior across all agent types.

This aligns with the Auto-Checkout on Wake decision in `product/task-system/auto-checkout/NODE.md`.
