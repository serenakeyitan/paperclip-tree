---
title: "Issue Graph Liveness"
owners: [bingran-you, cryppadotta, serenakeyitan]
soft_links: [engineering/backend/heartbeat-run-orchestration/NODE.md, product/agent-model/NODE.md, product/task-system/NODE.md, product/task-system/issue-blockers/NODE.md, product/task-system/run-liveness-continuations/NODE.md]
---

# Issue Graph Liveness

Issue graph liveness is a cross-issue health check that detects deadlocks in the blocker graph and escalates them to managers. It is distinct from run liveness, which classifies the outcome of a single heartbeat run. Where run liveness asks "did this run make progress?", graph liveness asks "can this issue ever make progress given the current state of its blockers and assignees?".

## Key Decisions

### Four Deadlock States

`classifyIssueGraphLiveness` (in `server/src/services/issue-liveness.ts`) emits findings in one of four states:

- `blocked_by_unassigned_issue` — a blocker has no assignee, so it cannot progress.
- `blocked_by_uninvokable_assignee` — the blocker's assignee is an agent that is not in an invokable status (e.g. archived, disabled).
- `blocked_by_cancelled_issue` — a blocker was cancelled, so the blocked issue will wait forever.
- `invalid_review_participant` — a review-state issue has a participant that cannot act on it.

Each finding carries a severity (`warning` or `critical`) so callers can decide whether to escalate immediately or surface as advisory.

### Reconciliation Runs at Startup and Periodically

`heartbeatService.reconcileIssueGraphLiveness` is invoked from `startServer` both at boot and on the periodic heartbeat-recovery tick. When findings produce escalations, the count is logged at warn level. This mirrors the existing stranded-issue reconciliation pattern, so graph liveness piggybacks on the same recovery cadence rather than introducing a separate scheduler.

### Escalations Surface as Manager Wakeups + Comments

When the reconciler finds a deadlock, it creates a manager escalation: a wakeup request against the responsible manager agent (via `reportsTo`) plus an issue comment describing the finding. The pure classifier in `issue-liveness.ts` is decoupled from persistence — it takes plain inputs (issues, relations, agents) and returns findings — which keeps it unit-testable without a database (see `issue-liveness.test.ts`).

## Relationship to Other Liveness Concepts

- **Run liveness** (`product/task-system/run-liveness-continuations`) classifies a single run's outcome and is metadata on `heartbeat_runs`.
- **Issue blockers** (`product/task-system/issue-blockers`) define the relationship type that graph liveness traverses.
- Graph liveness consumes the blocker graph and agent invokability state to detect when the graph as a whole is stuck.
