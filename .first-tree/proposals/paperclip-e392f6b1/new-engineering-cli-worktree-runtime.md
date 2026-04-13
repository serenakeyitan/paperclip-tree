---
type: TREE_MISS
source_id: paperclip-e392f6b1
source_commit_range: a3e125f79659e9d6a2caac8ff3a0eb3cd4127039..d6b06788f6efacb002791c1a60b4889d7bfdb22d
target_node: new
rationale: The CLI node mentions a 'worktree' command in one line but the tree has no node explaining the worktree runtime system — how agents execute in isolated git worktrees, how project environment variables propagate into worktree workspaces, or the hardening/reliability decisions around setup and teardown.
---
# Worktree Runtime

The worktree runtime manages git worktree-based execution workspaces for AI agents. When an agent is assigned a task, Paperclip can create an isolated git worktree so the agent works on a separate copy of the repository without interfering with the main checkout or other agents.

Source: `/cli/src/commands/worktree/` and related runtime setup in the server and adapter layers.

## How It Works

When a task run begins, the runtime creates a git worktree from the project's repository, sets up the execution environment (environment variables, credentials, project configuration), and hands the worktree path to the adapter as the agent's working directory. On completion or failure, the worktree is cleaned up.

## Key Decisions

- **Git worktrees over full clones.** Worktrees share the `.git` directory with the main checkout, making creation fast and disk-efficient. This is critical when multiple agents work on the same repo concurrently.
- **Project environment propagation.** Environment variables and project-level configuration (`.env`, YAML config) must be explicitly propagated into the worktree context. The runtime handles this rather than expecting adapters to discover env files themselves.
- **Hardened setup and teardown.** Worktree creation and cleanup are defensive — validating that the target branch exists, the worktree path is writable, and cleanup runs even on adapter crash or timeout. This prevents orphaned worktrees from accumulating.
- **Cross-cutting concern.** The worktree runtime spans CLI (user-facing commands), backend (task orchestration triggers worktree creation), and adapters (receive the worktree path as their `cwd`). The CLI owns the `worktree` subcommand; the backend owns the lifecycle orchestration.
