---
title: "Heartbeat Recovery-Origin Blocking"
owners: [bingran-you, cryppadotta, serenakeyitan]
soft_links: [engineering/backend/heartbeat-run-orchestration/NODE.md, product/task-system/run-liveness-continuations/NODE.md, product/task-system/stranded-issue-recovery/NODE.md]
---

# Heartbeat Recovery-Origin Blocking

When the heartbeat loop encounters an issue whose `originKind` is `stranded_issue_recovery`, it skips automatic recovery attempts and blocks the issue immediately, surfacing it for human or upstream resolution rather than spinning more recovery work on top of recovery work.

## Key Decisions

### Recovery-Origin Issues Block Immediately

The heartbeat's `shouldBlockImmediately` predicate now treats `RECOVERY_ORIGIN_KINDS.strandedIssueRecovery` as an unconditional block trigger, alongside the existing checks for missing/uninvokable recovery agents and prior recovery failures. Without this guard, a failing recovery issue could itself spawn another recovery issue, creating an unbounded recovery chain. The classifier `isStrandedIssueRecoveryOriginKind` makes this origin check reusable across recovery code paths.

### Concurrent Run Floor Lowered to 1

`normalizeMaxConcurrentRuns` previously clamped to a minimum of `HEARTBEAT_MAX_CONCURRENT_RUNS_DEFAULT`. It now clamps to `HEARTBEAT_MAX_CONCURRENT_RUNS_MIN = 1`, allowing operators to throttle an agent to a single concurrent run. Test coverage verifies that with `maxConcurrentRuns = 1`, a second assignment wake stays queued while the first run is in flight rather than executing in parallel.

## Why This Matters

Recovery is a last-resort mechanism. Letting recovery issues themselves trigger recovery defeats the safety net and can mask the underlying failure. Routing them straight to a blocked state forces the failure to surface instead of being papered over.
