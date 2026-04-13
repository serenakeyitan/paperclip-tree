---
title: "Execution Workspaces"
owners: [@bingran-you, @serenakeyitan, @bingran-you, @serenakeyitan]
---

# Execution Workspaces

How Paperclip provisions, manages, and reuses isolated work environments (git worktrees) for agent execution.

**Source:** `server/src/routes/execution-workspaces/`, `server/src/services/execution-workspace*`, `packages/shared/src/execution-workspace-guards.ts`, CLI `worktree` command.

---

## Architecture

### Worktree-Based Isolation

Each agent execution runs in a dedicated git worktree linked to the target repository. This provides filesystem isolation between concurrent agents without full repository clones. Worktrees are managed as first-class entities in the database (`execution_workspaces`, `workspace_operations`, `workspace_runtime_services` tables).

### Scoped Checkout

When an agent wakes, it checks out the issue's worktree with exclusive access. The checkout ties together the task system's atomic checkout (409 on conflict) with a physical worktree, ensuring no two agents modify the same files.

### Linked Worktree Reuse

Worktrees are reusable across agent wake cycles. When an agent resumes work on the same issue, it reuses the existing linked worktree rather than creating a new one — preserving uncommitted state and avoiding redundant setup.

### Environment Isolation

Environment variables and project configuration are explicitly propagated into the worktree context, not inherited from the parent checkout. This prevents cross-contamination between the dev runner's host environment and agent execution environments, and ensures each worktree gets a clean, scoped set of env vars.

---

## Key Decisions

- **Worktrees over full clones.** Git worktrees share the object store with the main repo, making creation fast and space-efficient when many agents run concurrently.
- **Reuse by default.** Linked worktrees persist between agent wakes to avoid redundant setup and preserve in-progress work.
- **Explicit env propagation over inheritance.** The runtime explicitly builds the worktree environment rather than inheriting from the parent process, preventing host environment leakage.
- **Cross-cutting concern.** Execution workspaces span CLI (user-facing `worktree` subcommand), backend (task orchestration triggers worktree creation), and adapters (receive the worktree path as their `cwd`).

---

## Boundaries

- Execution workspaces handle **filesystem isolation only**. Container isolation, network sandboxing, and resource limits belong to infrastructure/deployment.
- The task system owns **logical checkout** (who is assigned). Execution workspaces own **physical checkout** (where the work happens).
