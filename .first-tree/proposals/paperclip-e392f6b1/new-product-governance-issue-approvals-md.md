---
type: TREE_MISS
source_id: paperclip-e392f6b1
source_commit_range: a3e125f79659e9d6a2caac8ff3a0eb3cd4127039..d6b06788f6efacb002791c1a60b4889d7bfdb22d
target_node: new
rationale: The governance node lists fine-grained per-action approval gates as deferred, but this PR implements issue-level approvals — a third approval type beyond hiring and CEO strategy — with MCP-accessible creation and board approval card UI.
---
Issue-level approval gates extend the governance model beyond the V1 hiring and CEO strategy approvals. Any issue can now have an approval request attached, requiring Board sign-off before the agent proceeds.

## How It Works

Approval requests can be created on issues through the Board UI or programmatically via the MCP `approval-create` tool. This means external agents connected through adapters can request approval before proceeding with high-stakes actions — closing the loop between autonomous operation and human oversight.

Approval cards are rendered on the board with dedicated styling. The approval lifecycle (pending → approved/rejected) follows the same synchronous blocking pattern as hiring approvals: the requesting agent blocks until the Board decides.

## Relationship to Governance Model

This is the first implementation of the "fine-grained per-action governance gates" listed as deferred in the governance node. It moves issue-level approval from deferred to shipped.

## Technical Surface

The database schema includes `issue_approvals` alongside the existing `approvals` table. The MCP server (`engineering/mcp-server`) exposes the `approval-create` tool. The frontend renders approval cards inline on the issue board.
