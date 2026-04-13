---
title: "Execution Workspaces"
owners: [@bingran-you, @serenakeyitan, @bingran-you, @serenakeyitan]
---

# Execution Workspaces

How Paperclip provisions, manages, and reuses isolated work environments (git worktrees) for agent execution.

**Source:** `server/src/routes/execution-workspaces/`, `server/src/services/execution-workspace*`, `packages/shared/src/execution-workspace-guards.ts`, CLI `worktree` command.

## Architecture

Each agent execution runs in a dedicated git worktree linked to the target repository, providing filesystem isolation between concurrent agents without full repository clones. Worktrees are managed as first-class entities in the database (`execution_workspaces`, `workspace_operations`, `workspace_runtime_services` tables).

When an agent wakes, it checks out the issue's worktree with exclusive access (409 on conflict). Worktrees are reusable across agent wake cycles, preserving uncommitted state and avoiding redundant setup. A reseed mechanism refreshes an existing worktree to a newer base branch without full teardown.

## Key Decisions

- **Worktrees over full clones.** Git worktrees share the object store with the main repo, making creation fast and space-efficient when many agents run concurrently.
- **Reuse by default.** Linked worktrees persist between agent wakes to preserve in-progress work and avoid redundant checkout overhead.
- **Explicit environment propagation.** The runtime explicitly builds the worktree environment (`.env`, project config, credentials) rather than inheriting from the parent process, preventing host environment leakage into agent workspaces.
- **Hardened setup and teardown.** Worktree creation and cleanup are defensive — validating branch existence, path writability, and ensuring cleanup runs even on adapter crash or timeout to prevent orphaned worktrees.
- **Cross-cutting concern.** Execution workspaces span CLI (user-facing `worktree` subcommand), backend (task orchestration triggers workspace creation), and adapters (receive the worktree path as their working directory). The CLI owns the command surface; the backend owns lifecycle orchestration.

## Boundaries

Execution workspaces handle **filesystem isolation only**. Container isolation and resource limits belong to infrastructure/deployment. The task system owns **logical checkout** (who is assigned); execution workspaces own **physical checkout** (where the work happens).
