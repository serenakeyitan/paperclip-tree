---
title: "Routine Dispatch Fingerprint"
owners: [bingran-you, cryppadotta, serenakeyitan]
soft_links: ["product/routines", "product/task-system"]
---

# Routine Dispatch Fingerprint

Routine runs now carry a `dispatch_fingerprint` that distinguishes logically separate dispatches of the same routine, and the issues they create carry a matching `origin_fingerprint` used for open-issue deduplication.

**Source:** `server/src/services/routine*`, migration `0062_routine_run_dispatch_fingerprint.sql`.

## Key Decisions

### Fingerprint on the Run and the Issue

`routine_runs.dispatch_fingerprint` records the dispatch identity of a routine trigger. The derived issue stores `origin_fingerprint` (defaulting to `'default'` for backfill compatibility) so routine→issue lineage is preserved by fingerprint, not just routine id.

**Rationale:** A single routine can intentionally dispatch several parallel work streams (for example, per-target or per-shard work). Without a fingerprint, every dispatch collapses onto the same routine id and cannot be distinguished for dedup, lineage, or history queries. The `'default'` backfill value keeps existing routines working without migration of historical data.

### Open-Routine-Execution Uniqueness Includes Fingerprint

The `issues_open_routine_execution_uq` unique index keys on `(company_id, origin_kind, origin_id, origin_fingerprint)` for open routine-execution issues. Distinct fingerprints coexist as open issues; duplicates within a single fingerprint are still prevented.

**Rationale:** The previous uniqueness key prevented any routine from having more than one open issue at a time, which blocked parallel dispatch patterns. Including `origin_fingerprint` in the key preserves the anti-duplication guarantee within a single logical dispatch while allowing routines that fan out across fingerprints to run concurrently.

### Index for Fingerprint Lookup

`routine_runs_dispatch_fingerprint_idx` on `(routine_id, dispatch_fingerprint)` supports fast lookup from routine + fingerprint to the corresponding run history.

**Rationale:** Fingerprint-keyed lookup is on the hot path for dedup checks and for operator queries that trace a specific dispatch across runs. A dedicated index keeps these lookups from scanning the full routine history.

## Relationships

Dispatch fingerprint sits between `product/routines` (which defines routine lifecycle and concurrency policies) and `product/task-system` (which owns issue identity and dedup). The fingerprint is the join key that lets concurrency policies be expressed per-dispatch rather than per-routine.
