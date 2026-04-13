---
title: "Developer Tooling"
owners: [@bingran-you, @serenakeyitan, @bingran-you, @serenakeyitan]
---

Developer experience tooling, local development workflows, and worktree management for the Paperclip monorepo.

**Source:** `scripts/`, CLI `worktree` commands, `CONTRIBUTING.md`, development documentation.

## Worktree Tooling

Paperclip uses git worktrees as the primary isolation mechanism for concurrent agent and human development. The CLI provides worktree management commands that handle creation, linking to agent sessions, and cleanup. Worktree tooling bridges the gap between the task system's logical assignment and the physical filesystem workspace where agents execute.

## Key Decisions

- **Worktree-first development model.** Both human developers and AI agents use worktrees for isolated work, unifying the development model rather than having separate workflows.
- **CLI as the primary dev tool surface.** Developer tooling is exposed through the `paperclipai` CLI rather than separate scripts, keeping the tool surface unified and discoverable.
- **Contributor documentation lives in-repo.** Onboarding guides, setup instructions, and contribution workflows are maintained alongside the code they describe.

## Boundaries

This node covers developer-facing tooling and workflows. Agent execution workspace lifecycle is a related but distinct concern. CI/CD automation lives in `infrastructure/ci-cd`.
