---
type: TREE_SUPPLEMENT
source_id: paperclip-e392f6b1
source_commit_range: 45ebecab5a3d404970f555d1750dc73cf2b3a2be..5d1ed71779df5622d9fd99ad28816b2da4bdee31
target_node: new
rationale: The auto-checkout node describes harness-level checkout but doesn't mention the linked worktree reuse improvement or that checkout now ties into execution workspace reuse — it should cross-reference the new engineering/execution-workspaces node.
---
Add a soft_link to `engineering/execution-workspaces/NODE.md` and add a note under Harness-Level Automation:

### Workspace Continuity

When auto-checkout targets an issue that was previously worked on, the harness reuses the existing linked worktree rather than creating a fresh one. This preserves uncommitted changes and build state across wake cycles. See [Execution Workspaces](../../../engineering/execution-workspaces/NODE.md) for the full worktree lifecycle.
