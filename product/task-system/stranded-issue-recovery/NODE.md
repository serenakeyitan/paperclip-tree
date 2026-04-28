---
title: "Stranded Issue Recovery"
owners: [bingran-you, cryppadotta, serenakeyitan]
soft_links: [engineering/backend/heartbeat-run-orchestration/NODE.md, engineering/database/NODE.md, product/task-system/NODE.md, product/task-system/run-liveness-continuations/NODE.md]
---

# Stranded Issue Recovery

When a run fails or is orphaned and the heartbeat loop cannot automatically continue the originating issue, Paperclip creates a dedicated **recovery issue** with `originKind = 'stranded_issue_recovery'`. These issues are first-class work items owned by the recovery service rather than ad-hoc retries on the source issue.

## Key Decisions

### One Active Recovery Per Source Issue

A partial unique index `issues_active_stranded_issue_recovery_uq` on `(company_id, origin_kind, origin_id)` (filtered to `origin_kind = 'stranded_issue_recovery'`, non-hidden, status not in `done`/`cancelled`) guarantees that each stranded source issue has at most one active recovery issue at a time. This prevents duplicate recovery work from racing recovery passes or re-entrant heartbeat ticks.

### Stranded Recovery Issues Block Immediately

In the heartbeat scheduler, an issue whose `originKind` is `stranded_issue_recovery` short-circuits the normal recovery-agent-invokable check and is treated as `shouldBlockImmediately`. Recovery work is human/board-owned, not automatically retried by the runtime.

### Classifier Boundary

`isStrandedIssueRecoveryOriginKind` and the `RECOVERY_ORIGIN_KINDS.strandedIssueRecovery` constant in `server/src/services/recovery/` are the single source of truth for identifying recovery-owned issues. Other origins (`harness_liveness_escalation`, `manual`, etc.) are not recovery-owned and follow normal scheduling.

## Why It Matters

Without the unique index and the immediate-block rule, a flapping run could spawn multiple parallel recovery issues or get auto-retried into the same failure mode. Treating recovery as its own origin kind keeps the audit trail clean and makes the human escalation path explicit.
