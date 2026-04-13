---
type: TREE_MISS
source_id: paperclip-e392f6b1
source_commit_range: 45ebecab5a3d404970f555d1750dc73cf2b3a2be..5d1ed71779df5622d9fd99ad28816b2da4bdee31
target_node: new
rationale: Auto-checkout of the scoped issue's branch on agent wake is a new harness-level behavior not documented in the agent model node.
---
# Auto-Checkout on Issue Wake

When an agent wakes for a scoped issue, the execution harness automatically checks out the git branch associated with that issue before the agent receives its task prompt.

## Key Decisions

### Harness-Level, Not Agent-Level

Auto-checkout runs in the harness before the agent starts, not as an agent action. This ensures the agent always begins in the correct workspace context without needing to understand checkout mechanics.

### Scoped Issues Only

Auto-checkout activates only for issue-scoped wakes. General agent wakes (health checks, idle polls) do not trigger checkout. The checkout target follows the issue's branch, not the agent's last-known branch state.
