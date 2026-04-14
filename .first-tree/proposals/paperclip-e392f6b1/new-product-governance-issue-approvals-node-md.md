---
type: TREE_MISS
source_id: paperclip-e392f6b1
source_commit_range: db4e1465517f6e96876dda85488d4ab7210412a1..5d1ed71779df5622d9fd99ad28816b2da4bdee31
target_node: new
rationale: PR #3000 implements issue-level approvals — a third approval type beyond the V1 hiring and CEO strategy approvals — with MCP-accessible creation and board approval card UI. The governance node lists this as deferred ('fine-grained per-action governance gates'); it is now shipped and needs its own node.
---
# Issue Approvals

Issue-level approval gates extend the governance model beyond the V1 hiring and CEO strategy approvals. Any issue can now have an approval request attached, requiring Board sign-off before the agent proceeds.

## How It Works

Approval requests can be created on issues through the Board UI or programmatically via the MCP `approval-create` tool. External agents connected through adapters can request approval before proceeding with high-stakes actions.

Approval cards are rendered on the board with dedicated styling. The approval lifecycle (pending → approved/rejected) follows the same synchronous blocking pattern as hiring approvals: the requesting agent blocks until the Board decides.

## Relationship to Governance Model

This is the first implementation of the 'fine-grained per-action governance gates' listed as deferred in the governance node. It moves issue-level approval from deferred to shipped.

## Technical Surface

The database schema includes `issue_approvals` alongside the existing `approvals` table. The MCP server exposes the `approval-create` tool. The frontend renders approval cards inline on the issue board.
