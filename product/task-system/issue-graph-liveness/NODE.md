---
title: "Issue Graph Liveness"
owners: [bingran-you, cryppadotta, serenakeyitan]
soft_links: ["product/task-system/issue-blockers/NODE.md", "product/agent-model/NODE.md"]
---

# Issue Graph Liveness

Detects deadlocks in the issue dependency graph where work cannot progress because a blocker is unassigned, has an uninvokable assignee, has been cancelled, or participates in an invalid review configuration. When a deadlock is detected, the system escalates to the assignee's manager so a human or manager agent can unblock the work.

## Key Decisions

### Liveness States

Four distinct liveness states are classified:
- `blocked_by_unassigned_issue` — a blocker has no assignee, so the blocked issue cannot progress.
- `blocked_by_uninvokable_assignee` — the blocker's assignee exists but cannot be invoked (e.g., disabled agent).
- `blocked_by_cancelled_issue` — a blocker was cancelled but the blocked issue still lists it as a dependency.
- `invalid_review_participant` — a review relationship references an agent/user that cannot participate.

Each state has a severity (`warning` or `critical`) that drives whether an escalation is auto-created.

### Escalation to Manager

When a deadlock is classified, the heartbeat service creates an escalation issue or comment assigned to the blocked agent's `reportsTo` manager. This keeps resolution inside the existing issue/approval infrastructure rather than introducing a new alerting channel.

### Reconciliation on Startup and Periodically

`heartbeat.reconcileIssueGraphLiveness()` runs at server startup and on the periodic heartbeat recovery tick. This ensures deadlocks are caught even if the triggering event (cancellation, assignee change) was missed live — the graph is re-scanned regularly against the current state of issues, relations, and agents.

### Pure Classifier, Impure Reconciler

`classifyIssueGraphLiveness` (in `services/issue-liveness.ts`) is a pure function over issue/relation/agent inputs, making it unit-testable without a database. The reconciliation side (query, escalation creation, activity log) lives in `heartbeat.ts` and composes the classifier with DB access.

## Relationship to Issue Blockers

Issue blockers (`product/task-system/issue-blockers/`) define the forward path: when all blockers transition to `done`, the dependent is woken. Issue graph liveness covers the failure path: when blockers can never transition to `done` (unassigned, uninvokable, cancelled), the system detects the stuck state and escalates instead of waiting indefinitely.
