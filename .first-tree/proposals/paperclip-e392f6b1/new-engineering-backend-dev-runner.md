---
type: TREE_MISS
source_id: paperclip-e392f6b1
source_commit_range: 45ebecab5a3d404970f555d1750dc73cf2b3a2be..5d1ed71779df5622d9fd99ad28816b2da4bdee31
target_node: new
rationale: The dev runner subsystem (worktree environment isolation, linked worktree reuse, workspace import bootstrap) is not documented — backend NODE.md only lists execution-workspaces as a route group with no details.
---
# Dev Runner

Manages git worktree-based execution workspaces for agent task execution. Each agent execution gets an isolated worktree with its own environment variables.

## Key Decisions

### Worktree Environment Isolation

Environment variables do not leak between worktrees. Each worktree bootstraps its own environment from the workspace configuration, preventing cross-task contamination. This was tightened after a bug where one agent's credentials were visible to another.

### Linked Worktree Reuse

When an agent wakes to continue work on the same issue, the dev runner reuses the existing linked worktree rather than creating a new one. This preserves uncommitted changes, build artifacts, and installed dependencies across wake cycles.

### Workspace Import Bootstrap

Before an agent receives its task prompt, the dev runner runs an import sequence that ensures the worktree has the correct dependencies installed and configuration files in place. This was fixed after a regression where imports failed due to incorrect path resolution.
