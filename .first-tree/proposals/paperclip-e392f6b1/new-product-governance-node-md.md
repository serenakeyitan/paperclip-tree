---
type: TREE_SUPPLEMENT
source_id: paperclip-e392f6b1
source_commit_range: db4e1465517f6e96876dda85488d4ab7210412a1..5d1ed71779df5622d9fd99ad28816b2da4bdee31
target_node: new
rationale: The PR adds task-level approval requests (issue_approvals) beyond the two hard-coded V1 gates (hiring, CEO strategy), expanding governance to support arbitrary approval workflows triggered by agents or MCP tools.
---
### Task-Level Approval Requests

Beyond the two V1 gates (hiring, CEO strategy), agents and external tools can now request approval on individual issues before proceeding with high-stakes actions. This generalizes the approval pattern from hard-coded gates to a flexible, task-scoped sign-off workflow. Approval requests are stored in a dedicated `issue_approvals` table and rendered as approval cards in the UI.

**Rationale:** Hard-coding every approval gate doesn't scale. Agents encounter diverse high-stakes moments (deploying to production, spending above a threshold, merging critical PRs) that warrant human sign-off but can't all be predicted at design time. Task-level approvals let the governance surface grow with usage.
