---
title: "Assigned Todo Recovery Dispatch"
owners: [bingran-you, cryppadotta, serenakeyitan]
soft_links: [engineering/backend/heartbeat-run-orchestration/NODE.md, product/task-system/NODE.md, product/task-system/run-liveness-continuations/NODE.md]
---

How the heartbeat recovery sweep rescues assigned todo issues that were never dispatched to their agent.

## The Gap

When an issue is assigned to an agent and put into the todo state, the system normally enqueues a wakeup so the agent can begin work. If that initial dispatch is lost (e.g. server crash mid-assignment, missed wake enqueue), the issue can sit in `todo` indefinitely with no run, no queued wake, and an idle or paused agent — invisible to existing stranded-run reconciliation, which only looks for issues that already had a run.

## Recovery Behavior

The `reconcileStrandedAssignedIssues` recovery sweep now detects this case and re-emits an initial assignment dispatch wakeup. For each assigned todo issue with no active run and no already-queued wakeup carrying that `issueId`, the sweep enqueues a wakeup with `source: "assignment"`, `reason: "issue_assigned"`, and `mutation: "assigned_todo_liveness_dispatch"` in the payload, attributed to the system actor.

The sweep result exposes a new `assignmentDispatched` counter alongside `dispatchRequeued`, `continuationRequeued`, and `escalated`. The startup and periodic heartbeat loops in `server/src/index.ts` treat a non-zero `assignmentDispatched` as a signal to log/emit reconciliation activity, the same way they react to the other recovery counters.

## Idempotency

Before enqueuing, the sweep checks for any existing queued `agent_wakeup_requests` row whose `payload ->> 'issueId'` matches the issue. This prevents duplicate dispatches when the sweep runs repeatedly while a wakeup is already pending. Agent status is considered (paused/idle/running) so dispatches are not re-issued for agents already actively working the issue.
