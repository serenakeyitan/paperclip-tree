---
type: TREE_SUPPLEMENT
source_id: paperclip-e392f6b1
source_commit_range: db4e1465517f6e96876dda85488d4ab7210412a1..5d1ed71779df5622d9fd99ad28816b2da4bdee31
target_node: new
rationale: Multiple CI fixes (support manifest changes without lockfile, drop lockfile changes from MCP PR, restore lockfile to PR base) reflect new CI policies for MCP-related PRs.
---
### MCP Manifest CI Handling

CI supports MCP manifest changes without requiring lockfile changes in the same PR. When an MCP PR includes lockfile modifications, CI restores the lockfile to the PR base to keep MCP-only changes clean. This prevents lockfile churn from blocking MCP tool additions.
