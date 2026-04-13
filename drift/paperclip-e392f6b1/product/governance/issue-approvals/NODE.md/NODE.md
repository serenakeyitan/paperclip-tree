---
title: "Issue Approvals"
owners: []
---

---
title: "Issue Approvals"
owners: [bingran-you, cryppadotta, serenakeyitan]
soft_links: ["product/governance/NODE.md", "product/task-system/NODE.md", "engineering/mcp-server/NODE.md"]
---

# Issue Approvals

Issue-level approval gates that require board sign-off before an issue can proceed to execution. This extends the governance approval system beyond the original V1 gates (hiring and CEO strategy approval).

---

## Key Decisions

### Issue Approval as a Governance Gate

Individual issues can be flagged for board approval before agents may begin execution. When an issue requires approval, it enters a pending-approval state and the assigned agent is blocked from transitioning it to `in_progress` until the board approves or denies. This mirrors the existing CEO strategy approval pattern but applies at the individual issue level.

**Rationale:** Some work items carry enough risk, cost, or strategic importance that the board should explicitly authorize them before agents begin. This fills the gap identified in the governance model's deferred features — fine-grained per-action governance gates.

### Shared Approval Infrastructure

Issue approvals use the same approval infrastructure as hiring and strategy approvals — the unified `approvals` route group, activity logging, and synchronous blocking behavior. Approvals can also be created via the MCP server's approval creation tool.

---

## Boundaries

- The approval infrastructure (API surface, lifecycle, audit logging) is documented in the parent [governance](../NODE.md) node. This node covers the issue-specific application.
- Which issues require approval — the criteria or configuration for flagging — is an open design question.

## Open Questions

- Criteria for automatically flagging issues for approval (cost threshold, label-based, priority-based)
- Whether agents can request approval proactively or only the board can flag issues
