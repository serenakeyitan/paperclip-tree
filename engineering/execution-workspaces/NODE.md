---
title: "Execution Workspaces"
owners: [@bingran-you, @cryppadotta, @serenakeyitan]
---

# Execution Workspaces

How Paperclip provisions, manages, and reuses isolated work environments (git worktrees) for agent execution.

**Source:** `server/src/routes/execution-workspaces/`, `server/src/services/execution-workspace*`, `packages/shared/src/execution-workspace-guards.ts`, CLI `worktree` command.

## Architecture

### Worktree-Based Isolation

Each agent execution runs in a dedicated git worktree linked to the target repository. This provides filesystem isolation between concurrent agents without full repository clones. Worktrees are managed as first-class entities in the database (`execution_workspaces`, `workspace_operations`, `workspace_runtime_services` tables).

### Scoped Wake Checkout

When an agent wakes (via heartbeat, comment, or direct invocation), it checks out the issue it will work on. This checkout is scoped — the agent gets exclusive access to that issue's worktree. The checkout ties together the task system's atomic checkout (409 on conflict) with a physical worktree, ensuring no two agents modify the same files.

### Environment Isolation

Worktree environment isolation is explicit: environment variables and project configuration are propagated into the worktree context rather than inherited from the parent checkout. This applies to both production agent runs and the dev runner — the dev runner's worktree environment is isolated from the host shell environment to prevent env variable leakage between the main checkout and worktree workspaces.

### Linked Worktree Reuse

Worktrees are not disposable — they can be reused across agent wake cycles. When an agent resumes work on the same issue, it reuses the existing linked worktree rather than creating a new one. This preserves uncommitted state, editor context, and avoids redundant clone/checkout overhead.

## Key Decisions

- **Worktrees over full clones.** Git worktrees share the object store with the main repo, making them fast to create and space-efficient. Critical when many agents run concurrently on the same repository.
- **Explicit env propagation, not inheritance.** Worktree processes receive a curated set of environment variables rather than inheriting the parent process's full env. This prevents accidental cross-contamination between the main checkout and worktree workspaces, including during dev runner use.
- **Reuse by default.** Linked worktrees persist between agent wakes to avoid redundant setup and preserve in-progress work.
- **Cross-cutting concern.** Execution workspaces span CLI (user-facing `worktree` subcommand), backend (task orchestration triggers worktree creation), and adapters (receive the worktree path as their `cwd`). The backend owns lifecycle orchestration; the CLI owns the management subcommand.
