---
title: "Issue Approvals"
owners: []
---

# Issue Approvals

Issue-level approval gates that require human (board) sign-off before an issue can proceed to execution. This extends the governance approval system beyond the original V1 gates (hiring and CEO strategy approval).

## Key Decisions

### Issue Approval as a Governance Gate

Certain issues can be flagged for board approval before agents may begin execution. When an issue requires approval, it enters a pending-approval state and the assigned agent is blocked from transitioning it to `in_progress` until the board approves. This mirrors the existing CEO strategy approval pattern but applies at the individual issue level.

**Rationale:** Some work items carry enough risk, cost, or strategic importance that the board should explicitly authorize them before agents begin. This fills the gap identified in the governance model's "deferred features" — fine-grained per-action governance gates beyond hiring and strategy approval.

### Approval Workflow Integration

Issue approvals use the same approval infrastructure as hiring and strategy approvals — the unified `approvals` route group, activity logging, and synchronous blocking behavior. Agents requesting approval on an issue block until the board grants or denies it.

## Boundaries

- The **approval infrastructure** (API surface, approval lifecycle, audit logging) is documented in `product/governance`. This node covers the **issue-specific** application of that infrastructure.
- **Which issues require approval** — the criteria or configuration for flagging issues — is an open design question.
