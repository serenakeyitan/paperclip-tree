---
type: TREE_MISS
source_id: paperclip-e392f6b1
source_commit_range: db4e1465517f6e96876dda85488d4ab7210412a1..5d1ed71779df5622d9fd99ad28816b2da4bdee31
target_node: new
rationale: The tree has no node for the execution workspace / worktree runtime system. This PR adds worktree reseed functionality, and the broader worktree lifecycle (creation, reuse, reseed, cleanup, environment propagation) is a cross-cutting concern spanning CLI, backend, and adapters that no existing node captures.
---
# Execution Workspaces

How Paperclip provisions, manages, and reuses isolated work environments (git worktrees) for agent execution.

**Source:** `server/src/routes/execution-workspaces/`, `server/src/services/execution-workspace*`, CLI `worktree` command.

## Architecture

### Worktree-Based Isolation

Each agent execution runs in a dedicated git worktree linked to the target repository. This provides filesystem isolation between concurrent agents without full repository clones. Worktrees are managed as first-class entities in the database.

### Linked Worktree Reuse and Reseed

Worktrees are not disposable — they can be reused across agent wake cycles. When an agent resumes work on the same issue, it reuses the existing linked worktree rather than creating a new one. A **reseed** operation resets a worktree to a clean state from a target branch, preserving the worktree allocation while refreshing its content. This avoids redundant clone/checkout overhead and enables long-lived agent workspaces.

### Runtime Isolation Hardening

Worktree isolation is defensive: environment variables and project configuration are explicitly propagated (not inherited from the parent checkout), worktree paths are validated as writable before execution, and cleanup runs even on adapter crash or timeout. This prevents orphaned worktrees from accumulating.

## Key Decisions

- **Worktrees over full clones.** Git worktrees share the object store with the main repo, making them fast to create and space-efficient. Critical when many agents run concurrently on the same repository.
- **Workspace lifecycle tied to task lifecycle.** A workspace is created when an agent claims a task and persists until the task reaches a terminal state or is explicitly cleaned up.
- **Reuse by default, reseed on demand.** Linked worktrees persist between agent wakes. Reseed resets content without destroying the workspace allocation.
- **Cross-cutting concern.** Execution workspaces span CLI (user-facing `worktree` subcommand), backend (task orchestration triggers worktree creation), and adapters (receive the worktree path as their `cwd`). The backend owns lifecycle orchestration; the CLI owns the management subcommand.

## Boundaries

- Execution workspaces handle **filesystem isolation only**. Container isolation, network sandboxing, and resource limits belong to infrastructure/deployment.
- The task system owns **logical checkout** (who is assigned). Execution workspaces own **physical checkout** (where the work happens).
