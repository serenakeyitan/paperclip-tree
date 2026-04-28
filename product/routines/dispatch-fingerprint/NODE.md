---
title: "Routine Dispatch Fingerprint"
owners: [bingran-you, cryppadotta, serenakeyitan]
soft_links: [engineering/database/NODE.md, product/routines/NODE.md, product/task-system/NODE.md]
---

# Routine Dispatch Fingerprint

Routine runs and the issues they create now carry a **dispatch fingerprint** that distinguishes logically separate dispatches sharing the same `(origin_kind, origin_id)` pair.

## Key Decisions

### Fingerprint On Both Sides

`routine_runs.dispatch_fingerprint` (nullable) records the fingerprint chosen at dispatch time. The downstream issue stores `issues.origin_fingerprint` (NOT NULL, default `'default'`) so legacy rows remain valid. The pair lets a single routine produce multiple concurrent open issues without colliding on the routine-execution uniqueness index.

### Open-Routine-Execution Uniqueness Includes Fingerprint

The partial unique index `issues_open_routine_execution_uq` was widened from `(company_id, origin_kind, origin_id)` to `(company_id, origin_kind, origin_id, origin_fingerprint)`, scoped to open routine-execution issues (`status in ('backlog','todo','in_progress','in_review','blocked')`, not hidden, with an `execution_run_id`). This preserves the "one open issue per routine dispatch" invariant while allowing distinct fingerprints to coexist.

### Default Fingerprint Preserves Old Behavior

Routines that do not set a fingerprint inherit `'default'` on the issue side, so the new column is a strict superset of the old behavior — unfingerprinted dispatches still de-duplicate exactly as before.

## Why This Matters

The fingerprint is the routine layer's escape hatch for legitimate parallel dispatches (e.g. per-target, per-shard) that previously would have been suppressed by the unique index as duplicates of an already-open issue.
