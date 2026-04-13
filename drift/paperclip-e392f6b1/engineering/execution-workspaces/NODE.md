---
title: "Execution Workspaces"
owners: [@bingran-you, @serenakeyitan, @bingran-you, @serenakeyitan]
---

How Paperclip provisions, manages, and reuses isolated work environments (git worktrees) for agent execution.

**Source:** `server/src/routes/execution-workspaces/`, `server/src/services/execution-workspace*`, `packages/shared/src/execution-workspace-guards.ts`, CLI `worktree` command.

## Architecture

Each agent execution runs in a dedicated git worktree linked to the target repository, providing filesystem isolation between concurrent agents without full repository clones. Worktrees are managed as first-class entities in the database. When an agent wakes, it checks out the issue's worktree with exclusive access (409 on conflict). Worktrees are reusable across agent wake cycles, preserving uncommitted state.

## Key Decisions

- **Worktrees over full clones.** Git worktrees share the object store with the main repo, making creation fast and space-efficient when many agents run concurrently.
- **Reuse by default.** Linked worktrees persist between agent wakes to avoid redundant setup and preserve in-progress work.
- **Explicit env propagation over inheritance.** The runtime explicitly builds the worktree environment rather than inheriting from the parent process, preventing host environment leakage.
- **Cross-cutting concern.** Execution workspaces span CLI, backend, and adapters.

## Boundaries

Execution workspaces handle filesystem isolation only. Container isolation and resource limits belong to infrastructure/deployment. The task system owns logical checkout (who is assigned); execution workspaces own physical checkout (where the work happens).
