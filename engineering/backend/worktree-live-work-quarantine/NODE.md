---
title: "Seeded Worktree Live-Work Quarantine"
owners: [bingran-you, cryppadotta, serenakeyitan]
soft_links: ["engineering/execution-workspaces/NODE.md", "engineering/cli/NODE.md", "engineering/backend/heartbeat-run-orchestration/NODE.md"]
---

# Seeded Worktree Live-Work Quarantine

When a worktree is seeded from a live source instance via `paperclipai worktree init`, `reseed`, or `repair`, Paperclip quarantines copied live execution state by default for both `minimal` and `full` seed modes. This prevents a freshly booted isolated worktree from resuming agent work that is still owned by the source instance.

## Key Decisions

### Quarantine by Default

During restore, the CLI disables copied agent timer heartbeats, resets copied `running` agents to `idle`, blocks and unassigns copied agent-owned `in_progress` issues, and unassigns copied agent-owned `todo` and `in_review` issues. The seed flow summarizes these actions back to the operator, including counts such as disabled timer heartbeats and reset running agents.

**Rationale:** A seeded worktree is meant to be an isolated sandbox. If it inherits live assignments and timers unchanged, it can silently race the source instance, causing duplicate runs, conflicting commits, or duplicate issue comments. Safe isolation is therefore the default behavior.

### `--preserve-live-work` Is an Explicit Opt-In

Operators who intentionally want a seeded worktree to resume copied assignments can pass `--preserve-live-work` to `worktree init`, `reseed`, or `repair`. There is no global configuration switch for this behavior; preserving copied live work always requires an explicit per-command opt-in.

### Recovery Does Not Re-Link Seeded In-Progress Work

Heartbeat orphaned-process recovery does not auto-resume seeded `in_progress` issues that no longer have run linkage. The quarantine clears `checkoutRunId` and `executionRunId`, and the recovery path only re-links issues that still have a real active or legacy execution run to attach to.

**Rationale:** Quarantine and recovery must agree on the same safety invariant. Once copied live work has been intentionally detached during seeding, backend recovery should not reconstruct that linkage behind the operator's back.

## Boundaries

- `engineering/execution-workspaces` covers the broader worktree lifecycle and operator workflow.
- `engineering/cli` covers the CLI command surface that applies this quarantine during init, reseed, and repair.
- `engineering/backend/heartbeat-run-orchestration` covers the backend recovery behavior that respects the detached seeded state.

## Source

- `cli/src/commands/worktree.ts` — seeded-worktree quarantine and `--preserve-live-work`
- `server/src/services/heartbeat.ts` — execution recovery logic that only re-links issues with real run linkage
- `doc/DEVELOPING.md` — operator-facing worktree seeding documentation
