---
title: "Issue Approvals & MCP Governance Tool"
owners: [bingran-you, cryppadotta, serenakeyitan]
---

# Issue Approvals & MCP Governance Tool

Per-issue approval gates and an MCP tool for external agents to programmatically request approvals.

## Key Decisions

### Fine-Grained Per-Issue Approvals

Beyond the V1 gates (hiring, CEO strategy), any issue can now have an approval gate. Agents can request Board approval on specific issues, and the requesting agent blocks until the Board approves or rejects. This extends governance from coarse checkpoints to fine-grained, issue-level control.

**Rationale:** As autonomous agents take on more complex workflows, the two V1 gates are insufficient. Agents need a way to request human sign-off on individual pieces of work — code reviews, budget decisions, risky operations — without requiring new governance gate types for each scenario.

### MCP Approval Creation Tool

The MCP server exposes an `approval-create` tool that allows external agents (Claude Desktop, IDE extensions, MCP-compatible clients) to programmatically request approval on any issue. This uses the existing approval infrastructure — board powers, approval cards, and audit trail.

**Rationale:** MCP is the standard protocol for tool integration with AI agents. Exposing approval creation via MCP means any MCP-compatible agent can participate in Paperclip's governance model without needing direct API integration.
