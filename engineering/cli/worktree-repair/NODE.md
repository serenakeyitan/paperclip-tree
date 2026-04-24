---
title: "Worktree Repair"
owners: [bingran-you, cryppadotta, serenakeyitan]
soft_links: ["engineering/execution-workspaces/NODE.md", "engineering/backend/dev-runner/NODE.md"]
---

# Worktree Repair

How the CLI repairs or bootstraps Paperclip metadata for linked git worktrees
without mutating the primary checkout.

`paperclipai worktree repair` complements `worktree init`, `worktree make`, and
`worktree reseed` by handling the case where a linked worktree already exists on
disk but its Paperclip-managed sidecar state is missing or damaged.

## Key Decisions

### No-Op on Primary Checkout Without `--branch`

When invoked from the primary git checkout, `worktree repair` deliberately does
nothing unless `--branch` is provided. Repairing the primary checkout in place
would write worktree-local Paperclip metadata into the shared repo root, which
conflicts with the linked-worktree isolation model used for agent execution.

Requiring `--branch` makes the caller choose or create a linked worktree target
under `.paperclip/worktrees/` instead of silently mutating the primary clone.

### Repair the Current Linked Worktree by Default

When invoked inside a linked worktree and its Paperclip metadata is missing,
`worktree repair` repairs that worktree in place. This is the main recovery
path for cases where the git worktree still exists but `.paperclip/config.json`,
repo-local `.paperclip/.env`, or related sidecar state has been lost.

This preserves the existing git worktree and restores its Paperclip-local
runtime wiring instead of forcing operators or agents to recreate the workspace
from scratch.

### Branch Selectors May Resolve or Create a Linked Worktree

With `--branch`, the command first tries to resolve an existing linked worktree
by path, directory name, or branch name. If no registered linked worktree
matches, it creates a new git worktree under `.paperclip/worktrees/` and then
repairs or seeds its Paperclip metadata there.

This makes `worktree repair` safe to use both as a recovery tool for known
worktrees and as a bootstrap path when the caller knows the desired branch but
the linked worktree has not been recreated yet.

### Branch Names Are Validated Before Use

Branch names passed through `--branch` are validated with
`git check-ref-format --branch` before any worktree is created or repaired.
Invalid names fail fast with a clear error instead of producing an ambiguous
filesystem path or partially repaired workspace.

## Boundaries

- This node covers the CLI semantics for recovering worktree-local Paperclip
  metadata.
- The broader model for linked worktree isolation, reuse, and wake-scoped
  execution lives in [Execution Workspaces](../../execution-workspaces/NODE.md).
- Dev-time runner behavior that uses these worktrees lives in
  [Dev Runner](../../backend/dev-runner/NODE.md).
