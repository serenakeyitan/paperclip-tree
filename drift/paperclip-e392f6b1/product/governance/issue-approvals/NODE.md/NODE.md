---
title: "Issue-Level Approvals"
owners: []
---

Issue-level approval gates allow specific issues to require board or designated-authority approval before work proceeds. This extends the existing governance approval infrastructure (previously limited to hiring and CEO strategy decisions) to individual work items.

### Same Approval Infrastructure

Issue approvals reuse the existing approval system (board powers, approval cards, audit trail) rather than introducing a separate mechanism. This keeps governance consistent across all approval types.

### MCP-Accessible

External agents can create approval requests programmatically via the MCP `approval-create` tool, enabling automated governance workflows without direct API access.

### Relationship to Existing Gates

Issue approvals complement but do not replace the V1 hiring and CEO strategy gates. They operate at a finer granularity — per-issue rather than per-category — and can be triggered by agents or humans.
