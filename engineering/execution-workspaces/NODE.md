---
title: "Execution Workspaces"
owners: [bingran-you, cryppadotta, serenakeyitan]
---

# Execution Workspaces

How agents get isolated work environments via git worktrees, and how those worktrees are linked to tasks throughout the agent lifecycle.

## Key Decisions

### Git Worktree Isolation

Each agent execution runs in a dedicated git worktree rather than sharing a single clone. This prevents agents working on different issues from interfering with each other's file changes. Worktrees are managed by the server and exposed via the `execution-workspaces` API routes.

### Scoped Wake Checkout

When an agent boots (via heartbeat or wake-up), it checks out the issue it will work on through an API call that grants exclusive access. This scoped checkout ties the agent's worktree to a specific issue, ensuring no two agents operate on the same issue simultaneously. The checkout is atomic — if another agent already holds the issue, the request fails with a conflict.

### Linked Worktree Reuse

Worktrees that were previously created for an issue are reused when the same issue is picked up again, rather than creating a fresh worktree each time. This preserves in-progress work (uncommitted changes, build artifacts, agent session state) across agent wake cycles. The linkage between worktrees and issues is tracked in the `execution_workspaces` database table.

**Rationale:** Creating a new worktree on every wake would discard context and force agents to re-derive state. Reuse makes agent restarts cheap and preserves continuity, which is critical for long-running tasks that span multiple heartbeat cycles.

### Worktree Runtime Hardening

PR #2435 in the source repo introduced hardening to make worktree execution more reliable during comment-driven wakes. Key hardening measures:

- **Stale lock detection:** Worktrees left in a locked state from a previous crashed execution are detected and forcibly released rather than blocking new checkouts indefinitely.
- **Race condition guards:** Concurrent wake signals for the same issue (e.g., multiple rapid comments) are deduplicated at the checkout level — only one wake proceeds to execution, others are dropped or queued based on concurrency policy.
- **Cleanup on abort:** If an agent's worktree session exits abnormally, the workspace release is guaranteed via a server-side abort handler, preventing orphaned locks.

**Rationale:** Comment wake introduces event-driven concurrency patterns not present in scheduled heartbeats. Hardening the runtime ensures the execution workspace layer can handle rapid-fire wakes without leaving orphaned state.

## Boundaries

- Worktree creation and git operations are the server's responsibility, not the adapter's.
- Adapters receive the worktree path as `cwd` in their session params and work within it.
- Workspace cleanup (pruning stale worktrees) is a server-side concern tracked via `workspace_operations`.
