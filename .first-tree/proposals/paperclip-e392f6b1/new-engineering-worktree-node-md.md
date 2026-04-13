---
type: TREE_MISS
source_id: paperclip-e392f6b1
source_commit_range: 4d61dbfd34389a4723d6ae3df22df53501b42c71..11de5ae9c9523064a290755c02fb66e4e1c1b1e3
target_node: new
supersedes: null
rationale: Extensive worktree tooling was developed — reseed command, linked worktree reuse, env isolation, workspace link preflight, auto-checkout for scoped issue wakes, and conflict handling — representing a significant runtime subsystem.
---
## Overview

Worktrees provide isolated Git working directories for agent execution. Paperclip manages worktree lifecycle including creation, reuse, environment bootstrapping, and cleanup.

## Key Decisions

- **Reseed command** — A dedicated CLI command reseeds worktrees, refreshing their state without full recreation.
- **Linked worktree reuse** — Execution workspaces reuse existing linked worktrees when available, avoiding unnecessary cloning. Reuse logic handles edge cases like stale links.
- **Environment isolation** — Dev runner worktrees get isolated environment bootstrapping to prevent cross-contamination between agent sessions. Local `node_modules` are provisioned per worktree.
- **Auto-checkout on scoped wakes** — When an agent is woken for a specific issue, the harness automatically checks out the issue's branch in the worktree, eliminating the need for agents to call the checkout API themselves.
- **Conflict handling** — Checkout conflicts during harness auto-checkout are handled gracefully rather than failing the wake.
- **Workspace link preflight** — Workspace links are validated during dev preflight, scoped only to linked worktrees to avoid false positives.
