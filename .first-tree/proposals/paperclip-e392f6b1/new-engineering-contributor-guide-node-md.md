---
type: TREE_MISS
source_id: paperclip-e392f6b1
source_commit_range: 5e65bb2b92ae765815b6816cef60c25cdda837ca..7f893ac4ec9f700efaf902be8a57ce510c1c7092
target_node: new
rationale: No existing node covers contributor documentation or developer onboarding — the tree has no node for how new contributors get started, contribution workflows, or dev setup guides.
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
- Worktree mechanics are documented in `../execution-workspaces/NODE.md` and `../backend/dev-runner/NODE.md`.
