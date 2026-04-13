---
type: TREE_MISS
source_id: paperclip-e392f6b1
source_commit_range: db4e1465517f6e96876dda85488d4ab7210412a1..5d1ed71779df5622d9fd99ad28816b2da4bdee31
target_node: new
rationale: No tree node captures the execution workspace / worktree runtime system — how agents get isolated worktrees, how project environment variables propagate, and the hardening patterns for setup/teardown. The backend and CLI nodes mention worktrees in passing but don't cover the architecture or decisions.
---
# Execution Workspaces

How Paperclip provisions, manages, and reuses isolated work environments (git worktrees) for agent execution.

**Source:** `server/src/routes/execution-workspaces/`, `server/src/services/execution-workspace*`, CLI `worktree` command.

## Architecture

### Worktree-Based Isolation

Each agent execution runs in a dedicated git worktree linked to the target repository. This provides filesystem isolation between concurrent agents without full repository clones. Worktrees are managed as first-class entities in the database.

### Linked Worktree Reuse and Reseed

Worktrees are not disposable — they can be reused across agent wake cycles. When an agent resumes work on the same issue, it reuses the existing linked worktree rather than creating a new one. A **reseed** operation resets a worktree to a clean state from a target branch, preserving the worktree allocation while refreshing its content.

### Runtime Setup Hardening

Worktree creation and cleanup are defensive: branch existence and path writability are validated before execution, and cleanup runs unconditionally — even on adapter crash or timeout — to prevent orphaned worktrees from accumulating.

### Project Environment Propagation

Environment variables and project configuration (`.env`, YAML config) are explicitly propagated into worktree contexts rather than inherited from the parent checkout. This prevents accidental cross-contamination between agent sessions and ensures each agent sees the correct project-scoped configuration.

## Key Decisions

- **Worktrees over full clones.** Git worktrees share the object store with the main repo, making them fast to create and space-efficient. Critical when many agents run concurrently on the same repository.
- **Explicit env propagation over inheritance.** Adapters receive a fully resolved environment rather than discovering env files themselves, preventing configuration drift.
- **Reuse by default, reseed on demand.** Linked worktrees persist between agent wakes. Reseed resets content without destroying the workspace allocation.
- **Cross-cutting concern.** Execution workspaces span CLI (user-facing `worktree` subcommand), backend (task orchestration triggers worktree creation), and adapters (receive the worktree path as their `cwd`). The backend owns lifecycle orchestration; the CLI owns the management subcommand.

## Boundaries

- Execution workspaces handle **filesystem isolation only**. Container isolation, network sandboxing, and resource limits belong to infrastructure/deployment.
- The task system owns **logical assignment** (who is assigned). Execution workspaces own **physical checkout** (where the work happens).
