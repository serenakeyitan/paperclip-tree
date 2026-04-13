---
type: TREE_MISS
source_id: paperclip-e392f6b1
source_commit_range: 45ebecab5a3d404970f555d1750dc73cf2b3a2be..5d1ed71779df5622d9fd99ad28816b2da4bdee31
target_node: new
rationale: Execution workspaces (git worktree isolation, worktree-task linkage, and runtime service management) are referenced across CLI, backend, and database nodes but have no dedicated node documenting the architecture and key decisions.
---
# Execution Workspaces

How agents get isolated work environments via git worktrees, and how those worktrees are linked to tasks throughout the agent lifecycle.

## Key Decisions

### Git Worktree Isolation

Each agent execution runs in a dedicated git worktree rather than sharing a single clone. This prevents agents working on different issues from interfering with each other's file changes. Worktrees are managed by the server and exposed via the `execution-workspaces` API routes.

### Worktree Environment Isolation

Each worktree-based execution workspace bootstraps its own isolated environment variables. The dev runner scopes env per worktree rather than inheriting the parent process environment, preventing cross-task env leakage.

### Linked Worktree Reuse

Worktrees previously created for an issue are reused when the same issue is picked up again, preserving in-progress work across agent wake cycles. The linkage between worktrees and issues is tracked in the `execution_workspaces` database table.

### Workspace Import Bootstrap

New worktrees go through a bootstrap sequence that imports workspace-level configuration (dependencies, tool paths, adapter settings). The import must complete before the agent begins work — partial bootstrap is treated as a failure.
