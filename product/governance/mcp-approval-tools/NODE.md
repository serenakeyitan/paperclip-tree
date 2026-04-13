---
title: "MCP Approval Tools"
owners: [bingran-you, cryppadotta, serenakeyitan]
---

# MCP Approval Tools

Governance actions — specifically approval creation — are now exposed as MCP tools, allowing external agents and MCP clients to programmatically request approvals within the governance framework.

## Key Decisions

### Approval Creation via MCP

An MCP tool allows agents to create approval requests directly, rather than requiring them to go through the UI or a custom API integration. This lowers the barrier for agents to participate in governance workflows — an agent that determines it needs human sign-off can request it in the same protocol it uses for all other Paperclip interactions.

### Governance Constraints Still Enforced Server-Side

The MCP approval tool does not bypass any governance rules. The server-side approval service still enforces budget limits, required approver roles, and audit logging regardless of whether the request originated from the UI, API, or MCP. The MCP layer is a surface, not a policy layer.
