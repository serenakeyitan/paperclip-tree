---
type: TREE_MISS
source_id: paperclip-e392f6b1
source_commit_range: db4e1465517f6e96876dda85488d4ab7210412a1..5d1ed71779df5622d9fd99ad28816b2da4bdee31
target_node: new
rationale: A pending proposal already flagged this gap: the tree has no node explaining the worktree runtime system's environment propagation, defensive setup/teardown, or hardening decisions — the execution-workspaces node covers lifecycle and reuse but not the runtime reliability guarantees this PR introduced.
---
# Worktree Runtime

The worktree runtime manages git worktree-based execution workspaces for AI agents. When an agent is assigned a task, Paperclip creates an isolated git worktree so the agent works on a separate copy of the repository without interfering with the main checkout or other agents.

Source: `/cli/src/commands/worktree/` and related runtime setup in the server and adapter layers.

## How It Works

When a task run begins, the runtime creates a git worktree from the project's repository, sets up the execution environment (environment variables, credentials, project configuration), and hands the worktree path to the adapter as the agent's working directory. On completion or failure, the worktree is cleaned up.

## Key Decisions

### Git Worktrees Over Full Clones

Worktrees share the `.git` directory with the main checkout, making creation fast and disk-efficient. This is critical when multiple agents work on the same repo concurrently.

### Project Environment Propagation

Environment variables and project-level configuration (`.env`, YAML config) are explicitly propagated into the worktree context rather than inherited from the parent process. The runtime handles this — adapters should not discover env files themselves. This prevents environment leakage between worktrees and agents.

### Hardened Setup and Teardown

Worktree creation and cleanup are defensive: the runtime validates that the target branch exists and the worktree path is writable before execution begins. Cleanup runs even on adapter crash or timeout, preventing orphaned worktrees from accumulating.

### Cross-Cutting Concern

The worktree runtime spans CLI (user-facing `worktree` subcommand), backend (task orchestration triggers worktree creation), and adapters (receive the worktree path as their `cwd`). The CLI owns the `worktree` subcommand; the backend owns the lifecycle orchestration.

## Relationships

- **[Execution Workspaces](../../execution-workspaces/NODE.md)** — higher-level lifecycle (scoped checkout, linked reuse); worktree runtime is the underlying mechanism.
- **[Dev Runner](../../backend/dev-runner/NODE.md)** — local development variant with the same env isolation guarantees.
- **[Heartbeat Run Orchestration](../../backend/heartbeat-run-orchestration/NODE.md)** — triggers worktree creation as part of the run lifecycle.
