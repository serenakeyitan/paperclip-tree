---
type: TREE_MISS
source_id: paperclip-e392f6b1
source_commit_range: a3e125f79659e9d6a2caac8ff3a0eb3cd4127039..d6b06788f6efacb002791c1a60b4889d7bfdb22d
target_node: new
rationale: The tree has no node for execution workspace management — how git worktrees are created, linked to agent sessions, scoped to task checkout, and reused across wakes — despite it being a cross-cutting concern spanning backend, CLI, adapters, and the task system.
---
# Execution Workspaces

How Paperclip provisions, manages, and reuses isolated work environments (git worktrees) for agent execution.

**Source:** `server/src/routes/execution-workspaces/`, `server/src/services/execution-workspace*`, `packages/shared/src/execution-workspace-guards.ts`, CLI `worktree` command.

---

## Architecture

### Worktree-Based Isolation

Each agent execution runs in a dedicated git worktree linked to the target repository. This provides filesystem isolation between concurrent agents without full repository clones. Worktrees are managed as first-class entities in the database (`execution_workspaces`, `workspace_operations`, `workspace_runtime_services` tables).

### Scoped Wake Checkout

When an agent wakes (via heartbeat or direct invocation), it checks out the issue it will work on. This checkout is scoped — the agent gets exclusive access to that issue's worktree. The checkout ties together the task system's atomic checkout (409 on conflict) with a physical worktree, ensuring no two agents modify the same files.

### Linked Worktree Reuse

Worktrees are not disposable — they can be reused across agent wake cycles. When an agent resumes work on the same issue, it reuses the existing linked worktree rather than creating a new one. This preserves uncommitted state, editor context, and avoids redundant clone/checkout overhead.

---

## Key Decisions

- **Worktrees over full clones.** Git worktrees share the object store with the main repo, making them fast to create and space-efficient. This is critical when many agents run concurrently on the same repository.
- **Workspace lifecycle tied to task lifecycle.** A workspace is created when an agent claims a task and persists until the task reaches a terminal state or is explicitly cleaned up.
- **Reuse by default.** Linked worktrees persist between agent wakes to avoid redundant setup and preserve in-progress work.

---

## Boundaries

- Execution workspaces handle **filesystem isolation only**. They do not manage container isolation, network sandboxing, or resource limits — that belongs to infrastructure/deployment.
- The task system owns **logical checkout** (who is assigned). Execution workspaces own **physical checkout** (where the work happens).
