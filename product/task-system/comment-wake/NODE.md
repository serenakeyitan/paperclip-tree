---
title: "Comment Wake"
owners: [bingran-you, cryppadotta, serenakeyitan]
soft_links: ["engineering/frontend/issue-chat-composer-viewport"]
---

# Comment Wake

How posting a comment on a task triggers the assigned agent to wake up, and the efficiency considerations for this wake path.

## Key Decisions

### Comment-Driven Wake Trigger

When a comment is posted on a task (by a human or another agent), the system triggers a wake event for the assigned agent. This is the primary mechanism for interactive communication — it turns the task comment thread into a near-real-time conversation channel without requiring agents to poll.

### Wake Efficiency

Comment wakes are optimized for low latency. Rather than waiting for the next scheduled heartbeat, the system fires an immediate wake scoped to the commented issue. This ensures responsive agent behavior during active collaboration while avoiding unnecessary full-context rebuilds.

### Scoped Wake Payload

Comment wakes carry the issue scope, enabling downstream auto-checkout (see [Auto-Checkout on Wake](auto-checkout/NODE.md)) and worktree reuse (see [Execution Workspaces](../../engineering/execution-workspaces/NODE.md)). The wake payload is minimal — the agent fetches full context on boot rather than bundling it into the wake signal.

## Boundaries

- The **heartbeat protocol** (scheduled periodic wakes) is defined in [Agent Model](../agent-model/NODE.md). Comment wake is an event-driven complement to scheduled heartbeats.
- **Auto-checkout** behavior after wake is owned by [Auto-Checkout on Wake](auto-checkout/NODE.md).
- **Worktree isolation** during the resulting execution is owned by [Execution Workspaces](../../engineering/execution-workspaces/NODE.md).
- The **issue-chat stop-run control** complements comment wake by letting users interrupt an in-flight run from the transcript UI without changing the wake contract; that chat-surface UX is documented in `engineering/frontend/issue-chat-composer-viewport`.
