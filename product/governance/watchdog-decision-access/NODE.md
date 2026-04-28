---
title: "Watchdog Decision Access Gating"
owners: [bingran-you, cryppadotta, serenakeyitan]
soft_links: [engineering/backend/heartbeat-run-orchestration/NODE.md, engineering/frontend/issue-thread-ux/NODE.md, product/governance/NODE.md, product/governance/issue-approvals/NODE.md, product/task-system/run-liveness-continuations/NODE.md]
---

Stale-run watchdog decisions (continue monitoring, snooze, mark false positive) are gated by the viewer's board access. Only users with appropriate company membership can record watchdog decisions on a heartbeat run; non-owner viewers see the watchdog alert but no decision controls.

## Key Decisions

### Access-Gated Decision Controls

The `IssueRunLedger` component now accepts `canRecordWatchdogDecisions` and `watchdogDecisionError` props. When the viewer is a known non-owner (e.g., lacks the membership role required for the company that owns the issue), the decision buttons are hidden entirely rather than shown-and-rejected. This avoids dangling UI affordances that would only fail server-side.

### Membership Surfaced on Current Access

`/access/current` (see `server/src/routes/access.ts`) now returns a `memberships` array alongside `companyIds` and `isInstanceAdmin`, exposing each `{ companyId, membershipRole, status }` tuple. The frontend uses this to compute `canRecordWatchdogDecisions` for the issue's `companyId` rather than relying solely on the coarse `companyIds` membership list, which doesn't distinguish role or status.

### Evaluation Issue Assignee Threaded Through

The recovery service's `RunOutputSilenceSummary` and the shared `HeartbeatRunOutputSilence` type now carry `evaluationIssueAssigneeAgentId`. This lets the UI correlate the watchdog's evaluation issue with the agent expected to act on it, which is part of deciding whether the current viewer is an owner of the decision.

## How To Apply

When adding new decision-style controls that mutate run or issue governance state, gate the UI by the viewer's `memberships` (role + status for the relevant `companyId`) rather than just `companyIds`. Pipe `companyId` down from the issue page to any component that renders such controls so it can resolve the right membership entry.
