---
type: TREE_MISS
source_id: paperclip-e392f6b1
source_commit_range: db4e1465517f6e96876dda85488d4ab7210412a1..5d1ed71779df5622d9fd99ad28816b2da4bdee31
target_node: new
rationale: The governance node explicitly lists 'fine-grained per-action governance gates beyond hire and strategy approval' as deferred, but this PR ships issue-level approvals as a third approval type with MCP tool access and board UI rendering.
---
# Issue Approvals

Issue-level approval requests extend the governance model beyond the V1 hiring and CEO strategy gates. Any issue can now have an approval request attached, enabling structured sign-off workflows at the task level.

## Key Decisions

### Generalized Approval Pattern

Issue approvals follow the same synchronous blocking model as hiring and strategy approvals: the requesting agent blocks until the Board grants or denies approval. This reuses the existing governance enforcement at the server layer rather than introducing a separate mechanism.

**Rationale:** Consistency with existing approval gates reduces cognitive load and implementation surface. Agents interact with all approval types through the same patterns.

### MCP-Accessible Creation

Approval requests can be created programmatically via the MCP `approval-create` tool, not just through the Board UI. This allows agents connected through adapters to request approval before proceeding with high-stakes actions.

**Rationale:** Agents need to participate in governance workflows autonomously. Requiring UI interaction for approval creation would break the autonomous execution model.

### Database Schema

The `issue_approvals` table sits alongside the existing `approvals` table. Approval cards are rendered inline on the issue board with dedicated styling.

## Relationships

The MCP server (`engineering/mcp-server`) exposes the `approval-create` tool. The frontend renders approval cards on the board. This partially addresses the governance node's deferred item: 'fine-grained per-action governance gates.'
