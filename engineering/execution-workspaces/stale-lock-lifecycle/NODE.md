---
title: "Stale Execution Lock Lifecycle"
owners: [bingran-you, cryppadotta, serenakeyitan]
---

# Stale Execution Lock Lifecycle

How the system detects and recovers from stale execution locks — locks held by agents that have crashed, timed out, or otherwise failed to release their exclusive checkout.

## Key Decisions

### Stale Lock Detection

Execution locks can become stale when an agent process exits without releasing its scoped checkout (crash, OOM, adapter timeout). The server must detect these orphaned locks and reclaim them so the associated issues become available for reassignment. This is tracked as PIP-002.

### Lock Lifecycle States

Execution locks follow a defined lifecycle: acquisition (via scoped wake checkout), active hold (refreshed by heartbeats), and release (explicit on completion, or implicit via staleness detection). A lock that has not been refreshed within the staleness window is eligible for reclamation.

### Recovery Philosophy

Stale lock recovery aligns with the task system's principle that automatic recovery should not hide failures. When a stale lock is reclaimed, the system makes the event visible (via activity events) rather than silently reassigning the work. This ensures operators and manager agents can investigate why the lock went stale.

## Boundaries

- Lock lifecycle is a server-side concern — adapters do not manage lock state directly.
- Staleness detection is distinct from workspace cleanup (pruning worktrees); a lock can go stale while its worktree remains valid for reuse.
- The parent execution-workspaces node covers worktree creation and reuse; this node covers the failure-mode handling of the exclusive access mechanism.
