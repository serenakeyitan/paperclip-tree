---
title: "Issue Approvals"
owners: [@bingran-you, @cryppadotta, @serenakeyitan]
---

# Issue Approvals

Issue-level approval requests — a third approval type beyond the V1 hiring and CEO strategy gates. Approval requests can be created on issues through the Board UI or programmatically via the MCP `approval-create` tool.

## Key Decisions

### MCP-Accessible

External agents can create approval requests programmatically via the MCP `approval-create` tool, enabling automated governance workflows without requiring direct API access. This means adapter-connected agents can participate in governance workflows.

**Rationale:** Closing the loop between autonomous agent operation and human oversight requires that agents can request approval through the same protocol they use for other actions.

### Board Approval Cards

Approval requests render as dedicated cards on the issue board with purpose-built styling, making pending approvals visually distinct from regular issue activity.

## Schema

The database includes an `issue_approvals` table alongside the existing `approvals` table. The MCP server exposes the `approval-create` tool. The frontend renders approval cards inline on the issue board.

## Cross-Domain Links

- **MCP Server** (`engineering/mcp-server/`) — The `approval-create` tool is the programmatic interface for issue approvals.
- **Task System** (`product/task-system/`) — Approvals attach to issues in the task hierarchy.
