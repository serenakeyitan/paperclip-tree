# Dependency-Aware Heartbeat Scheduling

Heartbeat scheduling and issue auto-checkout are **blocker-aware**: an issue is not eligible to drive a heartbeat run or be auto-checked-out on wake while it still has unresolved `blockedByIssueIds`. This ties the blocker relation (product/task-system/issue-blockers) into the run orchestration layer (engineering/backend/heartbeat-run-orchestration) so agents don't burn runs on work whose prerequisites aren't done.

## Key Decisions

### Dependency Readiness as a First-Class Signal

The issues service exposes `IssueDependencyReadiness` ({ blockerIssueIds, unresolvedBlockerIssueIds, unresolvedBlockerCount, allBlockersDone, isDependencyReady }) via `getDependencyReadiness(issueId)` and a batch `listDependencyReadiness(companyId, issueIds)`. An issue with no blockers is trivially ready; otherwise it is ready only when every blocker is in `done` status. Source: `server/src/services/issues.ts`.

### Auto-Checkout Skips Blocked Issues

`shouldAutoCheckoutIssueForWake` now takes `isDependencyReady` and refuses to auto-check-out an issue for an agent wake when dependencies are unresolved, even if status/assignee conditions would otherwise match. Source: `server/src/services/heartbeat.ts`.

### Queued Heartbeat Runs Re-Ordered by Readiness

The heartbeat scheduler resolves dependency readiness for the issues referenced by queued runs' `contextSnapshot.issueId` and deprioritizes runs whose issue is not ready, falling back to issue priority rank (critical → low). This means blocked work sinks in the queue without being cancelled. Source: `server/src/services/heartbeat.ts` (`listQueuedRunDependencyReadiness`, `issueRunPriorityRank`).

### Agent Issue List Surfaces Readiness

The agent-facing issue list route (`agentRoutes`) now returns `dependencyReady`, `unresolvedBlockerCount`, and `unresolvedBlockerIssueIds` per issue so clients and adapters can reflect blocked state without a second round-trip. Source: `server/src/routes/agents.ts`.
