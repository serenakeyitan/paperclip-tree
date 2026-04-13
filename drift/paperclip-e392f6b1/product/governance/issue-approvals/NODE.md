---
title: "Issue Approvals"
owners: []
---

# Issue Approvals

Issue-level approval gates that require board sign-off before an issue can proceed to execution. This extends the governance approval system beyond the original V1 gates (hiring and CEO strategy approval).

## Key Decisions

### Issue Approval as a Governance Gate

Certain issues can be flagged for board approval before agents may begin execution. When an issue requires approval, it enters a pending-approval state and the assigned agent is blocked from transitioning it to `in_progress` until the board approves. This fills the gap identified in the governance model's open questions about fine-grained per-action gates.

**Rationale:** Some work items carry enough risk, cost, or strategic importance that the board should explicitly authorize them before agents begin. The existing hiring and CEO strategy gates proved the pattern; issue approvals generalize it.

### Shared Approval Infrastructure

Issue approvals reuse the same approval infrastructure as hiring and strategy approvals — the unified approvals API, activity logging, and synchronous blocking behavior. Approval status is visible on board/kanban cards.

## Boundaries

The approval infrastructure (API surface, lifecycle, audit logging) is documented in the parent [governance](../NODE.md) node. This node covers the issue-specific application of that infrastructure.

## Open Questions

- Criteria for automatically flagging issues for approval (cost threshold, label-based, priority-based)
- Whether agents can request approval proactively or only the board can flag issues
