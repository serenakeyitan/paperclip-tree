---
title: "Active-Run Output Watchdog"
owners: [bingran-you, cryppadotta, serenakeyitan]
soft_links: [engineering/backend/heartbeat-run-orchestration/NODE.md, engineering/database/NODE.md, product/task-system/NODE.md, product/task-system/run-liveness-continuations/NODE.md]
---

How the backend detects and escalates **silent active runs** — heartbeat runs whose process is still `running` but have stopped producing observable output. This is distinct from orphan-run reaping, queued-run resume, and stranded-work reconciliation; it covers the case where the process is alive but unproductive.

## Key Decisions

### Output Silence Is a Watchdog Signal, Not a Failure Verdict
The recovery service classifies active-run output silence as one of `ok`, `suspicious`, `critical`, `snoozed`, or `not_applicable`. Silence never directly cancels a run — it only drives whether a review issue is created and at what priority. `suspicious` creates a medium-priority review issue for the recovery owner; `critical` raises it to high priority and blocks the source issue on the evaluation task without killing the live process.

### Last-Output Tracking on Heartbeat Runs
`heartbeat_runs` gained `last_output_at`, `last_output_seq`, `last_output_stream`, and `last_output_bytes` columns, plus indexes on `(company_id, status, last_output_at)` and `(company_id, status, process_started_at)` to make the periodic scan cheap. These fields are the ground truth for output liveness and feed the `outputSilence` summary returned by live-run and active-run API responses.

### Single Open Evaluation Issue Per Run
A new origin kind `stale_active_run_evaluation` plus a partial unique index (`issues_active_stale_run_evaluation_uq`) guarantees at most one open evaluation issue per `(company_id, run_id)`. Snooze decisions in `heartbeat_run_watchdog_decisions` are honored before more review work is created, so operators can suppress noise without disabling the watchdog globally.

### Watchdog Decisions Are First-Class and Auditable
`heartbeat_run_watchdog_decisions` records every decision (`decision`, `snoozed_until`, `reason`, creator agent/user/run) with FKs to companies, runs, evaluation issues, and creator agents/runs. Cascading delete from `heartbeat_runs` keeps the table bounded; `set null` from issues/agents/runs preserves history when those entities are removed.

### Bounded, Redacted Evidence
Evidence collected for review issues (run logs, recent run events, child issues, blockers) is redaction-aware and truncated before being written to issue descriptions, so the watchdog never leaks secrets or unbounded log content into the issue surface.
