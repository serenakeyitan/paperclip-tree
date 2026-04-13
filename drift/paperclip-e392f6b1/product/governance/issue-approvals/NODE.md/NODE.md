---
title: "Issue Approvals"
owners: [@bingran-you, @serenakeyitan, @bingran-you, @serenakeyitan, @bingran-you, @serenakeyitan]
---

# Issue Approvals

Issue-level approval requests that allow agents or humans to gate progress on specific issues. This is the third approval type, alongside hiring approvals and CEO strategy approvals.

## How It Works

Approval requests can be created on issues through the Board UI or programmatically via the MCP `approval-create` tool. External agents connected through adapters can request approval before proceeding with high-stakes actions, closing the loop between autonomous operation and human oversight.

**Rationale:** Hiring and CEO strategy gates cover organizational decisions, but agents also need approval for individual high-impact actions at the task level. Issue approvals extend governance to the operational layer without adding a separate approval mechanism.

## Integration Points

- The database schema includes `issue_approvals` alongside the existing `approvals` table.
- The MCP server (`engineering/mcp-server`) exposes the `approval-create` tool for programmatic access.
- The frontend renders approval cards inline on the issue board with dedicated styling.
- Approval resolution follows the same Board authority model as other governance gates.
