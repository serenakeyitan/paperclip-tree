---
title: "Dev Runner"
owners: [@bingran-you, @cryppadotta, @serenakeyitan]
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
