---
title: "Stale Execution Lock Recovery"
owners: [bingran-you, cryppadotta, serenakeyitan]
soft_links: [engineering/backend/heartbeat-run-orchestration/NODE.md, engineering/execution-workspaces/NODE.md, product/governance/NODE.md, product/task-system/NODE.md]
---

# Stale Execution Lock Recovery

Issue checkout and execution-run locks can become stale when an agent harness crashes or disconnects without releasing them. The backend exposes recovery primitives that let operators and the system itself reconcile these locks without losing the audit trail.

## Key Decisions

### Board-Only Admin Force-Release Endpoint

`POST /issues/:issueId/admin/force-release` is an operator recovery endpoint for stale harness locks. It requires board access to the issue's company, clears `checkoutRunId`, `executionRunId`, and `executionLockedAt`, and optionally clears the agent assignee when `clearAssignee=true` is passed in the query string. Non-board actors receive `403`. This is intentionally a privileged escape hatch — routine release goes through the normal `/release` endpoint.

### Mandatory Audit Trail

Every force-release writes an `issue.admin_force_release` activity log entry that records the previous `checkoutRunId` and `executionRunId` before they were cleared, plus whether the assignee was cleared. This preserves forensic context for debugging why a lock was stuck and who broke it.

### Self-Healing Primitives in the Service Layer

Two service-level helpers complement the admin endpoint:

- `adoptUnownedCheckoutRun` lets a running agent reclaim an `in_progress` issue assigned to it whose `checkoutRunId` is null, by atomically setting checkout/execution run IDs to the actor's current run. Guarded by status, assignee, and run-id predicates so it cannot steal another agent's lock.
- `clearExecutionRunIfTerminal` releases the execution lock when the referenced heartbeat run has reached a terminal state, so a crashed run does not permanently block the issue.

These cover the common stale-lock cases automatically; the admin endpoint is the fallback when automatic recovery cannot decide safely.
