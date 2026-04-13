---
type: TREE_MISS
source_id: paperclip-e392f6b1
source_commit_range: db4e1465517f6e96876dda85488d4ab7210412a1..5d1ed71779df5622d9fd99ad28816b2da4bdee31
target_node: new
rationale: No tree node captures the runtime hardening patterns for heartbeat execution and adapter workflows — failure handling, retry, timeout, and defensive worktree management that bridge the agent model protocol and the adapter contract.
---
# Heartbeat & Adapter Runtime Hardening

Decisions and patterns for hardening the heartbeat execution and adapter runtime workflows.

## Context

The heartbeat protocol (see `product/agent-model/NODE.md`) and adapter execution contract (see `adapters/NODE.md`) define *what* heartbeats do and *how* adapters are invoked. This node captures the runtime hardening layer: how the system handles failures, retries, timeouts, and edge cases in those workflows.

## Key Patterns

- **Defensive worktree setup and teardown.** Worktree creation validates branch existence and path writability before execution. Cleanup runs unconditionally — even on adapter crash or timeout — to prevent orphaned worktrees.
- **Environment propagation.** Environment variables and project configuration are explicitly propagated into the worktree context rather than inherited, preventing accidental cross-contamination between agent sessions.
- **Graceful degradation.** When a heartbeat run encounters a transient failure, the system preserves the worktree state for retry rather than tearing it down, reducing redundant setup overhead.

## Boundaries

- Protocol-level heartbeat decisions remain in `product/agent-model/NODE.md`.
- Adapter contract details remain in `adapters/NODE.md`.
- This node captures the server-side runtime hardening patterns that bridge the two.
