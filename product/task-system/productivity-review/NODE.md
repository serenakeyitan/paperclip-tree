---
title: "Issue Productivity Review"
owners: [bingran-you, cryppadotta, serenakeyitan]
soft_links: [engineering/backend/heartbeat-recovery-blocking/NODE.md, engineering/backend/issue-routine-execution-filter/NODE.md, engineering/database/NODE.md, product/task-system/NODE.md]
---

# Issue Productivity Review

Productivity review is a new issue origin kind (`issue_productivity_review`) that Paperclip uses to spawn review issues tracking the productivity of other issues' work. It joins the existing built-in `ISSUE_ORIGIN_KINDS` alongside `manual`, `routine_execution`, `stale_active_run_evaluation`, `harness_liveness_escalation`, and `stranded_issue_recovery`.

## Key Decisions

### One Active Review Per Origin

The `issues_active_productivity_review_uq` unique partial index enforces that, per `(company_id, origin_kind, origin_id)`, at most one productivity-review issue can be active at a time — "active" meaning not hidden, not `done`, and not `cancelled`. This mirrors the pattern used for stranded-issue recovery: it prevents the system from piling up duplicate review issues against the same source issue while still allowing a new review to be opened once the previous one is resolved.

### Origin-Kind as First-Class Discriminator

Productivity review is modeled as an `originKind` value rather than a tag or status, so it participates uniformly in origin-aware list filtering, blocking rules, and recovery logic — the same machinery that handles routine-execution and recovery-origin issues applies here without special casing in the route layer.
