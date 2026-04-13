---
type: TREE_MISS
source_id: paperclip-e392f6b1
source_commit_range: db4e1465517f6e96876dda85488d4ab7210412a1..5d1ed71779df5622d9fd99ad28816b2da4bdee31
target_node: new
rationale: The governance node lists only two V1 approval gates (hiring and CEO strategy) and marks fine-grained gates as deferred, but this PR ships issue-level approval creation via MCP — a third approval type now exists.
---
# Issue Approvals

Issue-level approval requests that allow agents to request Board sign-off on specific pieces of work, beyond the V1 hiring and CEO strategy gates.

## Key Decisions

### MCP-Accessible Approvals

Issue approvals can be created through the MCP server's `approval-create` tool, enabling external agents to request human approval for governed actions on specific issues. This extends the approval system beyond internal adapter workflows.

### Board Approval Surface

Issue approval requests appear in the Board's approval queue alongside hiring and strategy approvals. The Board reviews, approves, or rejects them through the same unified governance interface.

**Rationale:** As agents take on more autonomous work, the set of decisions requiring human oversight expands beyond hiring and strategy. Issue-level approvals provide a general-purpose gate that agents can invoke when they encounter decisions that exceed their authority.
