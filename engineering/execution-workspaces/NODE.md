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

### Worktree Runtime-State Isolation

Execution worktrees carry their own dependency and environment state rather than inheriting the base repo's runtime state:

- **Worktree-local `node_modules`:** The provisioning script (`provision-worktree.sh`) installs dependencies into the worktree itself instead of symlinking to the base repo's `node_modules`. This prevents cross-worktree dependency pollution when different issues require different dependency states.
- **Repo-local `.paperclip/.env` bootstrap:** Each linked worktree has its own `.paperclip/.env` file containing `PAPERCLIP_HOME` and `PAPERCLIP_INSTANCE_ID`. The runtime reads env state from the worktree's local `.paperclip/` directory, not from the primary checkout.
- **Fail-fast on missing env:** If a linked worktree is missing its repo-local `.paperclip/.env` or the env file lacks the required fields, the runtime fails immediately rather than silently falling back to the base repo's configuration. This prevents agents from accidentally operating with the wrong instance identity.

**Rationale:** Filesystem isolation (separate git worktrees) is necessary but not sufficient — agents also need isolated runtime state. Without this, two worktrees sharing the same `node_modules` or `.env` can cause subtle cross-contamination bugs during concurrent execution.

## Boundaries

- Worktree creation and git operations are the server's responsibility, not the adapter's.
- Adapters receive the worktree path as `cwd` in their session params and work within it.
- Workspace cleanup (pruning stale worktrees) is a server-side concern tracked via `workspace_operations`.
