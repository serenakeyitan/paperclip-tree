---
title: "Deferred Comment Wakes During Active Runs"
owners: [bingran-you, cryppadotta, serenakeyitan]
soft_links: [engineering/backend/heartbeat-run-orchestration/NODE.md, product/governance/issue-approvals/NODE.md, product/task-system/comment-wake/NODE.md, product/task-system/run-liveness-continuations/NODE.md]
---

# Deferred Comment Wakes During Active Runs

When an issue receives an event that would normally wake its assignee (for example, an issue-approval being approved), and the assignee already has a heartbeat run in progress for that issue, Paperclip defers the wake instead of enqueuing a concurrent run.

## Key Decisions

### Defer, Don't Stack

If the assignee is currently running on the issue, the heartbeat service records the wake as deferred. When the active run finishes, the deferred wake is consumed and the assignee resumes — picking up the new context (e.g. the freshly granted approval) on the next heartbeat rather than racing the in-flight run.

**Rationale:** Stacking a second run on top of an active one wastes adapter time and can produce conflicting writes. Deferring keeps the run loop linear per issue while still guaranteeing the wake is honored.

### Approval-Approved Wakes Are Treated Like Comment Wakes

The deferral path covers approval-approved events alongside ordinary comment wakes, so governance gates clearing mid-run flow back to the assignee through the same wake-batching machinery rather than a separate side channel.
