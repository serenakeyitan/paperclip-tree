---
title: "Issue Approvals"
owners: [@bingran-you, @serenakeyitan, @bingran-you, @serenakeyitan, @bingran-you, @serenakeyitan]
---

# Issue Approvals

Fine-grained per-issue approval gates. A dedicated `issue_approvals` table sits alongside the existing `approvals` table, enabling approval workflows scoped to individual issues rather than only at the company or project level.

## Key Decisions

### Separate Table

Issue approvals use a dedicated `issue_approvals` table rather than overloading the existing `approvals` table. This keeps the schema clean and allows issue-specific query patterns without impacting existing approval flows.

### Multi-Channel Access

Issue approvals are accessible via the MCP `approval-create` tool (for agent-initiated approvals) and inline Board UI cards (for human review). This closes the governance node's long-standing deferred item for fine-grained per-action governance gates.
