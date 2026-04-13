---
type: TREE_MISS
source_id: paperclip-e392f6b1
source_commit_range: a3e125f79659e9d6a2caac8ff3a0eb3cd4127039..d6b06788f6efacb002791c1a60b4889d7bfdb22d
target_node: new
rationale: The PR adds issue-level approval creation (via MCP tool), expanding governance beyond the V1 hiring and CEO strategy approval gates — the governance node doesn't capture this third approval type.
---
## Issue Approvals

Issue-level approval requests extend the governance model beyond the original V1 gates (hiring approval, CEO strategy approval). Any issue can now have an approval request attached, enabling structured sign-off workflows at the task level.

### How It Works

Approval requests can be created on issues through the UI or programmatically via the MCP `approval-create` tool. This means external agents connected through adapters can participate in governance workflows — requesting approval before proceeding with high-stakes actions.

### Relationship to Existing Gates

V1 governance defined two hard-coded approval gates (hiring, CEO strategy). Issue approvals generalize this pattern: any issue can require approval, and the approval workflow follows the same audit trail and board-level visibility as the original gates.

### Cross-Domain Links

The MCP server (`engineering/mcp-server`) exposes the approval-create tool. The database schema includes `issue_approvals` alongside the existing `approvals` table. The frontend renders approval cards on the board with dedicated styling (polished in this PR).
