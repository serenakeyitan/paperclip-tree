---
type: TREE_MISS
source_id: paperclip-e392f6b1
source_commit_range: a3e125f79659e9d6a2caac8ff3a0eb3cd4127039..d6b06788f6efacb002791c1a60b4889d7bfdb22d
target_node: new
rationale: The tree has proposals for execution workspaces but no actual node. This PR further hardens worktree runtime isolation, reinforcing that execution workspaces are a cross-cutting architectural concern that deserves a first-class tree node covering worktree creation, scoped checkout, reuse, and isolation guarantees.
---
# Execution Workspaces

How Paperclip provisions, manages, and reuses isolated work environments (git worktrees) for agent execution.

**Source:** `server/src/routes/execution-workspaces/`, `server/src/services/execution-workspace*`, `packages/shared/src/execution-workspace-guards.ts`, CLI `worktree` command.

## Architecture

### Worktree-Based Isolation

Each agent execution runs in a dedicated git worktree linked to the target repository. This provides filesystem isolation between concurrent agents without full repository clones. Worktrees are managed as first-class entities in the database (`execution_workspaces`, `workspace_operations`, `workspace_runtime_services` tables).

### Scoped Wake Checkout

When an agent wakes (via heartbeat, comment, or direct invocation), it checks out the issue it will work on. This checkout is scoped — the agent gets exclusive access to that issue's worktree. The checkout ties together the task system's atomic checkout (409 on conflict) with a physical worktree, ensuring no two agents modify the same files.

### Linked Worktree Reuse

Worktrees are not disposable — they can be reused across agent wake cycles. When an agent resumes work on the same issue, it reuses the existing linked worktree rather than creating a new one. This preserves uncommitted state, editor context, and avoids redundant clone/checkout overhead.

### Runtime Isolation Hardening

Worktree isolation is defensive: environment variables and project configuration are explicitly propagated (not inherited from the parent checkout), worktree paths are validated as writable before execution, and cleanup runs even on adapter crash or timeout. This prevents orphaned worktrees from accumulating and ensures agents cannot accidentally interfere with each other's filesystems.

## Key Decisions

- **Worktrees over full clones.** Git worktrees share the object store with the main repo, making them fast to create and space-efficient. Critical when many agents run concurrently on the same repository.
- **Workspace lifecycle tied to task lifecycle.** A workspace is created when an agent claims a task and persists until the task reaches a terminal state or is explicitly cleaned up.
- **Reuse by default.** Linked worktrees persist between agent wakes to avoid redundant setup and preserve in-progress work.
- **Cross-cutting concern.** Execution workspaces span CLI (user-facing `worktree` subcommand), backend (task orchestration triggers worktree creation), and adapters (receive the worktree path as their `cwd`). The backend owns lifecycle orchestration; the CLI owns the management subcommand.

## Boundaries

- Execution workspaces handle **filesystem isolation only**. Container isolation, network sandboxing, and resource limits belong to infrastructure/deployment.
- The task system owns **logical checkout** (who is assigned). Execution workspaces own **physical checkout** (where the work happens).
