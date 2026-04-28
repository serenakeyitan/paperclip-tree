---
title: "Dependency-Aware Heartbeat Scheduling"
owners: [bingran-you, cryppadotta, serenakeyitan]
soft_links: [engineering/backend/heartbeat-run-orchestration/NODE.md, product/task-system/NODE.md, product/task-system/auto-checkout/NODE.md, product/task-system/issue-blockers/NODE.md]
---

# Dependency-Aware Heartbeat Scheduling

Heartbeat scheduling and issue auto-checkout now respect issue blocker relationships. An issue is **dependency-ready** only when all of its `blockedByIssueIds` are in `done` status; otherwise it is held back from being woken or auto-checked-out, even if it is otherwise eligible.

## Key Decisions

### Dependency Readiness as a First-Class Signal

The issues service exposes a `IssueDependencyReadiness` shape (`blockerIssueIds`, `unresolvedBlockerIssueIds`, `unresolvedBlockerCount`, `allBlockersDone`, `isDependencyReady`) via `getDependencyReadiness` and a batch `listDependencyReadiness`. The agent issues list endpoint hydrates each row with `dependencyReady`, `unresolvedBlockerCount`, and `unresolvedBlockerIssueIds` so clients and the scheduler share one definition of "ready to run."

### Scheduler Defers Blocked Runs

The heartbeat service computes dependency readiness for queued runs (keyed by the `issueId` in their context snapshot) and skips runs whose issue is not dependency-ready. Among ready runs, ordering still follows issue priority (`critical` → `high` → `medium` → `low` → unset). Blocked runs stay queued rather than failing; they become eligible automatically once their blockers reach `done`.

### Auto-Checkout Gated by Readiness

`shouldAutoCheckoutIssueForWake` now requires `isDependencyReady` in addition to assignee/status checks. A wake-up that targets a blocked issue will not auto-check it out, preventing agents from picking up work that cannot proceed.

## Why This Matters

Before this change, blocker relationships were modeled at the product layer but did not influence scheduling — agents could be woken on issues whose prerequisites were unfinished. Centralizing readiness in the issues service and consuming it from both the heartbeat scheduler and the agent issue list keeps the product-level blocker semantics and the runtime scheduling behavior in lockstep.
