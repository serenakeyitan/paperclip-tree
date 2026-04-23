---
title: "Stale Execution Lock Recovery"
owners: [bingran-you, cryppadotta, serenakeyitan]
soft_links: ["product/task-system/NODE.md", "product/governance/NODE.md"]
---

# Stale Execution Lock Recovery

Issues carry execution-run lock fields (`checkoutRunId`, `executionRunId`, `executionLockedAt`) that pair an in-progress issue with the harness run currently working it. When a harness crashes or a run terminates abnormally, these locks can get stranded and block further progress on the issue. Stale execution lock recovery is the set of behaviors that detect and resolve these strandings.

This node refines the parent [Task System](../NODE.md) "No Automatic Recovery" decision: Paperclip still does not auto-reassign work, but it does expose explicit, audited recovery paths so operators and healthy runs can unstick an issue without bypassing governance.

## Key Decisions

### Board-Only Admin Force-Release

A dedicated operator endpoint `POST /issues/:issueId/admin/force-release` clears stranded execution locks. It requires board access to the issue's company — this is a governance-sensitive recovery action, not a routine agent operation, because it overrides the normal checkout/release lifecycle. It optionally accepts `clearAssignee=true` to also detach the agent assignee when the agent itself is gone.

**Rationale:** Force-release overrides the atomic checkout contract that the rest of the task system depends on. Gating it behind board access keeps the governance surface small and ensures the humans (or principals) accountable for the company are the ones who approve overrides.

### Always Audit Force-Release

Every force-release writes an `issue.admin_force_release` activity log entry including the previous `checkoutRunId` and `executionRunId`. Recovery actions bypass the normal locking contract, so the audit trail is the only record of what state was overwritten — it must always be present.

**Rationale:** Without this log, a force-release is indistinguishable from a normal release, and there is no way to reconstruct which run was evicted or why. Aligned with the governance-first principle that every mutation has an audit trail.

### Terminal-Run Auto-Clear and Orphan Adoption

Alongside the manual operator path, the issue service exposes automatic recovery helpers:

- `clearExecutionRunIfTerminal` clears lock fields when the referenced heartbeat run has reached a terminal state.
- `adoptUnownedCheckoutRun` lets the rightful assignee agent re-claim an in-progress issue whose checkout lock has been cleared.

These let healthy runs self-heal without requiring board intervention for every stale lock.

**Rationale:** A terminated run cannot release its own lock, so trusting the heartbeat's terminal state is a safe, information-preserving signal. Orphan adoption keeps the single-assignee invariant intact while avoiding an operator ticket for every crashed run.

## Why This Belongs on the Tree

This is a governance-adjacent recovery surface that crosses the task-system, governance, and heartbeat-orchestration domains. Future agents designing features that touch issue checkout or execution runs must know this recovery path exists, that it is audited, and that the manual path is board-gated.

## Source

- paperclipai/paperclip#4258 — "[codex] Fix stale issue execution run locks" (introduced the operator endpoint, activity log entry, and auto-clear / adoption helpers)
