---
title: "Issue Approvals"
owners: []
---

## Issue Approvals

Issue-level approval requests extend the governance model beyond the original V1 gates (hiring approval, CEO strategy approval). Any issue can now have an approval request attached, enabling structured sign-off workflows at the task level.

### How It Works

Approval requests can be created on issues through the UI or programmatically via the MCP `approval-create` tool. This means external agents connected through adapters can participate in governance workflows — requesting approval before proceeding with high-stakes actions.

### Relationship to Existing Gates

V1 governance defined two hard-coded approval gates (hiring, CEO strategy). Issue approvals generalize this pattern: any issue can require approval, and the approval workflow follows the same audit trail and board-level visibility as the original gates.

### Cross-Domain Links

The MCP server (`engineering/mcp-server`) exposes the approval-create tool. The database schema includes `issue_approvals` alongside the existing `approvals` table. The frontend renders approval cards on the board with dedicated styling (polished in this PR).
