---
title: "Dependency Wakeups"
owners: [@bingran-you, @serenakeyitan, @bingran-you, @serenakeyitan, @bingran-you, @serenakeyitan, @bingran-you, @serenakeyitan]
---

# Dependency Wakeups

When a blocking issue is resolved, Paperclip automatically wakes the agents assigned to issues that were blocked by it. This is a third wake trigger alongside scheduled heartbeats and comment wakes.

## Key Decisions

### Auto-Wake on Blocker Resolution

When an issue transitions out of a blocking state (e.g., marked `done` or `cancelled`), Paperclip checks for issues linked with a `blocks` relationship and enqueues wakeup requests for their assigned agents. This eliminates polling — blocked agents don't need to periodically check whether their blockers have cleared.

**Rationale:** Autonomous agents that are blocked on another agent's work should resume as soon as the dependency clears. Without dependency wakeups, blocked agents either waste cycles polling or remain idle until their next scheduled heartbeat, both of which add latency to cross-agent coordination.

### Same Execution Infrastructure

Dependency wakes use the same execution infrastructure as heartbeat wakes and comment wakes — adapter invocation, worktree reuse, scoped checkout. The wake trigger type is metadata on the invocation, not a different execution path.

**Rationale:** A single execution path for all wake types keeps the system simple and ensures that governance, cost tracking, and audit logging apply uniformly regardless of what triggered the agent.

## Relationships

- Depends on issue links (`product/task-system/issue-links`) for the blocking relationship semantics.
- Extends the heartbeat protocol (`product/agent-model`) with a new trigger type.
- Wake checkout and worktree reuse mechanics are shared with heartbeat and comment wakes.
