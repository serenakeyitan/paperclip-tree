---
title: "Execution Workspaces"
owners: [@bingran-you, @serenakeyitan, @bingran-you, @serenakeyitan]
---

# Execution Workspaces

How Paperclip provisions, manages, and reuses isolated git worktree-based work environments for agent execution.

**Source:** `server/src/routes/execution-workspaces/`, `server/src/services/execution-workspace*`, `packages/shared/src/execution-workspace-guards.ts`, CLI `worktree` command.

## Architecture

Each agent execution runs in a dedicated git worktree linked to the target repository, providing filesystem isolation between concurrent agents without full repository clones. Worktrees are managed as first-class entities in the database. When an agent wakes, it checks out the issue's worktree with exclusive access (409 on conflict). Worktrees are reusable across agent wake cycles, preserving uncommitted state.

## Key Decisions

### Git Worktrees Over Full Clones

Worktrees share the `.git` object store with the main checkout, making creation fast and disk-efficient. This is critical when multiple agents work on the same repo concurrently.

### Reuse by Default

Linked worktrees persist between agent wakes to avoid redundant setup and preserve in-progress work. When an agent wakes to continue work on an issue it previously worked on, it reuses the existing worktree rather than creating a new one.

### Worktree Reseed

When the base branch has advanced, the runtime reseeds the worktree in place rather than destroying and recreating it. This preserves local state (environment setup, cached builds) while keeping the workspace current with upstream changes.

### Explicit Environment Propagation

The runtime explicitly builds the worktree environment rather than inheriting from the parent process, preventing host environment leakage. Environment variables and project-level configuration are propagated deliberately into the worktree context.

### Hardened Setup and Teardown

Worktree creation and cleanup are defensive — validating that the target branch exists, the worktree path is writable, and cleanup runs even on adapter crash or timeout. This prevents orphaned worktrees from accumulating.

## Boundaries

Execution workspaces handle filesystem isolation only. Container isolation and resource limits belong to `infrastructure/deployment`. The task system owns logical checkout (who is assigned); execution workspaces own physical checkout (where the work happens). This is a cross-cutting concern spanning CLI, backend, and adapters.
