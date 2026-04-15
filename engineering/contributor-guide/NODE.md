---
title: "Contributor Guide"
owners: [bingran-you, cryppadotta, serenakeyitan]
---

# Contributor Guide

Documentation and conventions for contributing to the Paperclip monorepo. Covers developer setup, contribution workflows, and project conventions.

## Key Decisions

### Contributor Docs Live in the Source Repo

Contributor-facing documentation (setup guides, contribution workflows, coding conventions) lives in the source repo as markdown files (e.g., CONTRIBUTING.md), not in the context tree. The tree captures the *decision* to maintain these docs and any conventions about their structure, but the docs themselves are maintained alongside the code they describe.

### Worktree-Based Development Workflow

Contributors working on Paperclip use git worktrees for isolated development environments. The contributor docs explain how to create, manage, and clean up worktrees as part of the standard development workflow.

## Boundaries

- Actual contributor documentation content lives in the source repo.
- This node captures decisions about what contributor docs cover and how they're structured.
- Agent execution worktrees (how agents get isolated environments at runtime) are a separate concern documented in `../execution-workspaces/NODE.md`.
- Dev runner tooling is documented in `../backend/dev-runner/NODE.md`.
