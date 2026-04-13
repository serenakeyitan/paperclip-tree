---
type: TREE_MISS
source_id: paperclip-e392f6b1
source_commit_range: 45ebecab5a3d404970f555d1750dc73cf2b3a2be..5d1ed71779df5622d9fd99ad28816b2da4bdee31
target_node: new
rationale: Auto-checkout of scoped issues when agents wake in the harness is a new orchestration behavior not captured in any existing node.
---
# Scoped Issue Auto-Checkout

When an agent wakes (resumes or is assigned work) in the execution harness, the system automatically checks out the git branch associated with the scoped issue. This removes a manual step from the agent workflow and ensures the agent starts in the correct workspace context.

## Key Decisions

### Automatic on Wake

Checkout happens as part of the wake sequence, before the agent receives its task prompt. This guarantees the agent's filesystem state matches the issue it is about to work on, preventing a class of bugs where agents operate on stale or wrong branches.

### Scoped to Issue, Not Agent

The checkout target is determined by the issue's associated branch, not the agent's last-known state. If an agent is reassigned to a different issue, it checks out the new issue's branch. This follows the task-system principle that issues own the work context, not agents.
