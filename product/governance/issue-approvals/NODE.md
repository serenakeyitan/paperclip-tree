---
title: "Issue Approvals"
owners: [bingran-you, cryppadotta, serenakeyitan]
---

# Issue Approvals

Issue-level approval gates allow specific issues to require human sign-off before work proceeds. This extends the existing governance approval infrastructure — previously limited to hiring and CEO strategy decisions — to individual work items.

## Key Decisions

### Reuse Existing Approval Infrastructure

Issue approvals reuse the same approval system (board powers, approval cards, audit trail) rather than introducing a separate mechanism. This keeps governance consistent across all approval types and avoids fragmenting the approval workflow.

### MCP-Accessible Approval Creation

External agents can create approval requests programmatically via the MCP `approval-create` tool, enabling automated governance workflows without direct API access. This means agents connected through any adapter can participate in governance — requesting approval before proceeding with high-stakes actions.

### Complement, Don't Replace V1 Gates

Issue approvals complement but do not replace the V1 hiring and CEO strategy gates. They operate at a finer granularity — per-issue rather than per-category — and can be triggered by agents or humans. The V1 gates remain as fixed, always-on governance checkpoints.

## Cross-Domain Links

- The MCP server (`engineering/mcp-server`) exposes the `approval-create` tool
- The database schema includes `issue_approvals` alongside the existing `approvals` table
- The frontend renders approval cards on the board with dedicated styling
- Governance enforcement follows the same server-layer pattern documented in `product/governance`
