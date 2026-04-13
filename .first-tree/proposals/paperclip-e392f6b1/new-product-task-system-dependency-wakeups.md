---
type: TREE_MISS
source_id: paperclip-e392f6b1
source_commit_range: db4e1465517f6e96876dda85488d4ab7210412a1..5d1ed71779df5622d9fd99ad28816b2da4bdee31
target_node: new
rationale: The PR introduces dependency wakeups — when a blocking issue resolves, dependent issues auto-wake their assigned agents. No existing node (including the drift issue-links node and agent-model heartbeat protocol) captures this new wake trigger type or its behavioral semantics.
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
