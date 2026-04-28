---
title: "Manual Routine Run Inbox Visibility"
owners: [bingran-you, cryppadotta, serenakeyitan]
soft_links: [engineering/backend/issue-routine-execution-filter/NODE.md, engineering/frontend/inbox-list/NODE.md, product/routines/NODE.md, product/task-system/NODE.md]
---

# Manual Routine Run Inbox Visibility

When a board user manually runs a routine, the resulting routine-execution issue must show up in that user's inbox even though routine-execution issues are otherwise treated as background noise. The routines service records the manual actor (agent or board user) on the freshly created issue and primes inbox state so the issue surfaces in the runner's inbox immediately.

## Key Decisions

### Pass Manual Actor Through `runRoutine`

The `POST /api/routines/:id/run` route now forwards the request actor (`agentId` for agent actors, `userId` for board actors) into `routineService.runRoutine` as a third argument. Previously the service had no notion of who triggered a manual run, so the resulting issue could not be tied back to a human inbox. The actor is captured at the route boundary so the service stays agnostic to Express request shapes.

### Touch Inbox State on Issue Creation

When a manual run creates a fresh routine issue, the service writes to `issue_read_states` and clears any matching `issue_inbox_archives` row for the manual user. This guarantees the issue appears unarchived and tracked in that user's inbox view from the moment it is created, rather than waiting for the user to interact with it. The write uses `onConflictDoUpdate` keyed on `(companyId, issueId, userId)` so re-runs are idempotent.

### Interaction with Routine-Execution Filtering

Issue lists hide `routine_execution`-origin issues by default unless the caller opts in (see `engineering/backend/issue-routine-execution-filter`). Manual-run inbox visibility is the counterweight: even with the global default hiding routine-execution noise, the specific human who triggered the run sees their own routine issue surface in their inbox via the touched read state.
