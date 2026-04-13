---
type: TREE_MISS
source_id: paperclip-e392f6b1
source_commit_range: db4e1465517f6e96876dda85488d4ab7210412a1..5d1ed71779df5622d9fd99ad28816b2da4bdee31
target_node: new
rationale: The CLI node mentions a worktree command in one line but the tree has no node explaining the worktree runtime system — how agents execute in isolated git worktrees, how environment variables propagate, or the hardening decisions around setup and teardown introduced by this PR.
---
# Worktree Runtime

The worktree runtime manages git worktree-based execution workspaces for AI agents. When an agent is assigned a task, Paperclip creates an isolated git worktree so the agent works on a separate copy of the repository without interfering with the main checkout or other agents.

**Source:** `/cli/src/commands/worktree/` and related runtime setup in the server and adapter layers.

## How It Works

When a task run begins, the runtime creates a git worktree from the project's repository, sets up the execution environment (environment variables, credentials, project configuration), and hands the worktree path to the adapter as the agent's working directory. On completion or failure, the worktree is cleaned up.

## Key Decisions

- **Git worktrees over full clones.** Worktrees share the `.git` directory with the main checkout, making creation fast and disk-efficient. Critical when multiple agents work on the same repo concurrently.
- **Project environment propagation.** Environment variables and project-level configuration (`.env`, YAML config) are explicitly propagated into the worktree context. The runtime handles this rather than expecting adapters to discover env files.
- **Hardened setup and teardown.** Worktree creation and cleanup are defensive — validating branch existence, worktree path writability, and ensuring cleanup runs even on adapter crash or timeout. Prevents orphaned worktrees from accumulating.
- **Cross-cutting concern.** The worktree runtime spans CLI (user-facing commands), backend (task orchestration triggers creation), and adapters (receive the worktree path as their `cwd`). The CLI owns the `worktree` subcommand; the backend owns lifecycle orchestration.
