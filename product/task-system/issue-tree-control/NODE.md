---
title: "Issue Tree Control (Pause / Cancel / Resume / Restore)"
owners: [bingran-you, cryppadotta, serenakeyitan]
soft_links: [engineering/backend/heartbeat-run-orchestration/NODE.md, engineering/database/NODE.md, product/governance/NODE.md, product/task-system/NODE.md, product/task-system/comment-wake/NODE.md, product/task-system/dependency-blocked-interaction/NODE.md, product/task-system/issue-blockers/NODE.md]
---

# Issue Tree Control

Issue tree control lets a human or agent pause, cancel, resume, or restore an entire issue subtree (a root issue and its descendants) as a single governance action. The subtree is materialized into a **hold** that records which issues are affected, who created it, and how it should be released.

## Key Decisions

### Holds Are First-Class, Persisted Records

Holds live in two tables: `issue_tree_holds` (one row per hold, with mode, status, actor attribution for create and release, and an optional `release_policy`) and `issue_tree_hold_members` (one row per affected issue, capturing the issue's identifier/title/status, parent, depth, current assignee, and active run at the time the hold was applied). This snapshot lets the system reason about the subtree even if individual issues change after the hold is created, and supports auditability — every hold records both the creator and (when released) the releaser, with optional reasons.

### Four Modes: pause, resume, cancel, restore

The control modes (`ISSUE_TREE_CONTROL_MODES`) are: `pause` (suppress normal wakeups across the subtree while keeping issue state — and today also interrupts active runs and cancels queued/deferred wakeups immediately when the hold is created, regardless of release strategy), `resume` (release an active pause), `cancel` (mark subtree issues as cancelled and tear down active runs / unclaimed wakeups), and `restore` (reverse a prior cancel by restoring issue statuses captured in the hold member rows). Cancel and restore are paired operations that rely on member-row snapshots to recover prior status.

### Preview Before Apply

Every control action is preview-able via `POST /issues/:id/tree-control/preview` before it is applied. Previews enumerate the affected issues, their depth, currently active runs, per-agent counts, totals, and warnings (e.g. issues already held). This lets the UI show a confirmation surface and lets agents make an informed decision before committing.

### Wake Semantics Under a Hold

The heartbeat service is hold-aware. While a `pause` hold is active on an issue, normal scheduled wakeups for that issue are suppressed, but **interaction wakes** (e.g. comment-driven wakes) still flow through so humans can engage. The wake payload exposes a `treeHoldInteraction` flag and an `activeTreeHold` summary (`holdId`, `rootIssueId`, `mode`, `reason`) so adapters can surface the held state to the agent. Cancel additionally cancels in-flight runs and unclaimed wakeups for the subtree.

### Release Policies

Holds carry an optional `release_policy` with strategy `manual` or `after_active_runs_finish`. Manual is the default — a hold persists until a release call arrives. The `after_active_runs_finish` strategy is recorded on the hold but, today, `pause` and `cancel` still interrupt active runs and cancel queued/deferred wakeups immediately when the hold is created (`server/src/routes/issue-tree-control.ts` lines 94-139). The strategy currently determines how the hold is released, not whether in-flight execution is preserved on creation. Treat behavioral preservation of in-flight runs under the deferred strategy as forward-looking, not yet implemented.

## Surfaces

- API:
  - `POST /issues/:id/tree-control/preview` — preview a control action.
  - `POST /issues/:id/tree-holds` — create a hold (apply pause/cancel/resume/restore).
  - `GET /issues/:id/tree-control/state` — current control state for an issue.
  - `GET /issues/:id/tree-holds` — list holds for an issue.
  - `GET /issues/:id/tree-holds/:holdId` — fetch a single hold.
  - `POST /issues/:id/tree-holds/:holdId/release` — release a hold.
- Validators: `previewIssueTreeControlSchema`, `createIssueTreeHoldSchema`, `releaseIssueTreeHoldSchema`.
- Service: `server/src/services/issue-tree-control.ts`.
- UI: tree-control affordances in `IssueDetail` and `IssuesList`, with hold awareness in `IssueChatThread`.
