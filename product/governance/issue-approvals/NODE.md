---
title: "Issue Approvals"
owners: [bingran-you, cryppadotta, serenakeyitan]
---

# Issue Approvals

Issue-level approval gates allow any issue to require explicit sign-off before work proceeds. This extends the V1 governance model (which only gated hiring and CEO strategy approval) to individual work items.

## Key Decisions

### Reuses Existing Approval Infrastructure

Issue approvals use the same approval system as hiring and strategy gates — board powers, approval cards, audit trail, and synchronous blocking. The requesting agent blocks until the Board approves or rejects. No separate mechanism was introduced.

### MCP-Accessible Creation

Approval requests can be created programmatically via the MCP `approval-create` tool, enabling agents connected through adapters to request governance sign-off without direct API access. This closes the loop between autonomous agent operation and human oversight.

### Complements V1 Gates

Issue approvals complement but do not replace the V1 hiring and CEO strategy gates. They operate at finer granularity — per-issue rather than per-category — and can be triggered by either agents or humans.

## Relationship to Governance Model

This is the first implementation of the "fine-grained per-action governance gates" previously listed as deferred in the parent governance node. The governance node's deferred list should be updated to reflect this.
