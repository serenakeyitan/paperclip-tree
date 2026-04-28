---
title: "Harness Liveness Escalation Issues"
owners: [bingran-you, cryppadotta, serenakeyitan]
soft_links: [engineering/backend/heartbeat-run-orchestration/NODE.md, product/task-system/NODE.md, product/task-system/run-liveness-continuations/NODE.md]
---

When the control plane detects lost execution continuity that it cannot recover automatically, it creates **harness liveness escalation issues** to surface the incident for explicit recovery or human escalation.

## Key Decisions

### One Open Incident Per Origin
A partial unique index (`issues_active_liveness_recovery_incident_uq`) on `(company_id, origin_kind, origin_id)` for `harness_liveness_escalation` ensures only one open escalation issue exists per origin. This keeps the recovery surface deduplicated across recovery passes and restart cycles.

### Leaf Fingerprint Dedupe
A second partial unique index (`issues_active_liveness_recovery_leaf_uq`) on `(company_id, origin_kind, origin_fingerprint)` — applied only when `origin_fingerprint <> 'default'` — deduplicates at a finer leaf granularity. The default fingerprint is excluded so generic incidents can coexist while specific fingerprinted incidents stay unique.

### Recovery Policy: Retry Once, Then Escalate
The SPEC recovery rule was tightened: control-plane recovery may retry lost execution continuity once, then falls back to explicit recovery issues or human escalation. Automatic reassignment is still excluded — escalation issues are the explicit handoff surface.
