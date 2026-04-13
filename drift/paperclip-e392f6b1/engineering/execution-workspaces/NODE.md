---
title: "Execution Workspaces"
owners: []
---

# Execution Workspaces

How Paperclip provisions, manages, and reuses isolated work environments (git worktrees) for agent execution.

**Source:** `server/src/routes/execution-workspaces/`, `server/src/services/execution-workspace*`, `packages/shared/src/execution-workspace-guards.ts`, CLI `worktree` command.

## Architecture

### Worktree-Based Isolation

Each agent execution runs in a dedicated git worktree linked to the target repository. This provides filesystem isolation between concurrent agents without full repository clones. Worktrees are managed as first-class entities in the database (`execution_workspaces`, `workspace_operations`, `workspace_runtime_services` tables).

### Scoped Wake Checkout

When an agent wakes (via heartbeat or direct invocation), it checks out the issue it will work on. This checkout is scoped — the agent gets exclusive access to that issue's worktree. The checkout ties together the task system's atomic checkout (409 on conflict) with a physical worktree, ensuring no two agents modify the same files.

### Linked Worktree Reuse

Worktrees are not disposable — they can be reused across agent wake cycles. When an agent resumes work on the same issue, it reuses the existing linked worktree rather than creating a new one. This preserves uncommitted state, editor context, and avoids redundant clone/checkout overhead.

### Worktree Reseed

Reseed resets an existing worktree to a clean state (typically the latest main branch) without destroying and recreating it. This is used when an agent needs a fresh starting point for a new attempt on the same task, or when stale worktree state would interfere with new work. Reseed preserves the worktree identity and database linkage while refreshing its filesystem contents.

## Key Decisions

- **Worktrees over full clones.** Git worktrees share the object store with the main repo, making them fast to create and space-efficient. This is critical when many agents run concurrently on the same repository.
- **Workspace lifecycle tied to task lifecycle.** A workspace is created when an agent claims a task and persists until the task reaches a terminal state or is explicitly cleaned up.
- **Reuse by default.** Linked worktrees persist between agent wakes to avoid redundant setup and preserve in-progress work.
- **Reseed over recreate.** When a workspace needs to be reset, reseed refreshes the content in-place rather than deleting and recreating the worktree, preserving the workspace identity and avoiding database churn.

## Boundaries

- Execution workspaces handle **filesystem isolation only**. They do not manage container isolation, network sandboxing, or resource limits — that belongs to infrastructure/deployment.
- The task system owns **logical checkout** (who is assigned). Execution workspaces own **physical checkout** (where the work happens).
