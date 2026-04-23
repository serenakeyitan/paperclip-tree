---
title: "Stale Execution Lock Recovery"
owners: [bingran-you, cryppadotta, serenakeyitan]
---

# Stale Execution Lock Recovery

This recovery surface is paperclip-specific and scoped to a single `company_id` — like every other task-system entity, issues and their execution-run locks never cross company boundaries, and every recovery action (manual or automatic) operates within the owning company's governance context.

Issues carry execution-run lock fields (`checkoutRunId`, `executionRunId`, `executionLockedAt`) that pair an in-progress issue with the harness run currently working it. When a harness crashes or a run terminates abnormally, these locks can get stranded and block further progress on the issue. Stale execution lock recovery is the set of behaviors that detect and resolve these strandings.

## Key Decisions

### Board-Only Admin Force-Release

A dedicated operator endpoint `POST /issues/:issueId/admin/force-release` exists to clear stranded execution locks. It requires board access to the issue's company — this is a governance-sensitive recovery action, not a routine agent operation, because it overrides the normal checkout/release lifecycle. Optionally accepts `clearAssignee=true` to also detach the agent assignee when the agent itself is gone.

### Always Audit Force-Release

Every force-release writes an `issue.admin_force_release` activity log entry including the previous `checkoutRunId` and `executionRunId`. Recovery actions bypass the normal locking contract, so the audit trail is the only record of what state was overwritten — it must always be present.

### Terminal-Run Auto-Clear and Orphan Adoption

Alongside the manual operator path, the issue service exposes automatic recovery helpers: `clearExecutionRunIfTerminal` clears lock fields when the referenced heartbeat run has reached a terminal state, and `adoptUnownedCheckoutRun` lets the rightful assignee agent re-claim an in-progress issue whose checkout lock has been cleared. These let healthy runs self-heal without requiring board intervention for every stale lock.

## Why This Belongs on the Tree

This is a governance-adjacent recovery surface that crosses the task-system, governance, and heartbeat-orchestration domains. Future agents designing features that touch issue checkout or execution runs must know this recovery path exists and that it is audited and board-gated.
