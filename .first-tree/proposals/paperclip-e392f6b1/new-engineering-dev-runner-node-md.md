---
type: TREE_MISS
source_id: paperclip-e392f6b1
source_commit_range: 45ebecab5a3d404970f555d1750dc73cf2b3a2be..5d1ed71779df5622d9fd99ad28816b2da4bdee31
target_node: new
rationale: The dev runner is an undocumented subsystem responsible for worktree-based execution workspace management, env isolation, and linked worktree reuse — none of which appear in any existing node.
---
# Dev Runner

The dev runner manages **execution workspaces** for agent tasks using Git worktrees. It handles workspace creation, environment bootstrap, and cleanup for agents that need an isolated checkout to perform work.

**Source:** Server-side execution infrastructure + CLI `worktree` command

## Key Decisions

### Worktree environment isolation

Each worktree-based execution workspace gets its own isolated environment variables. The dev runner bootstraps a scoped env for each worktree rather than inheriting the parent process environment wholesale. This prevents cross-task env leakage (e.g., one agent's API keys or workspace paths bleeding into another agent's execution context).

### Linked worktree reuse

When a task targets a branch that already has a linked worktree, the dev runner reuses the existing worktree rather than creating a fresh one. This avoids redundant checkouts and preserves in-progress state. The reuse logic validates that the existing worktree is in a clean state before attaching a new execution.

### Workspace import bootstrap

New worktrees go through a bootstrap sequence that imports workspace-level configuration (dependencies, tool paths, adapter settings) into the isolated environment. The import must complete before the agent begins work — partial bootstrap is treated as a failure.
