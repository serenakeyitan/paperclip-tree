---
title: "Heartbeat Run Summaries"
owners: [bingran-you, cryppadotta, serenakeyitan]
---

# Heartbeat Run Summaries

Server-side service that aggregates heartbeat run history into per-agent status snapshots, enabling efficient agent health displays without querying the full event log.

**Source:** `server/src/services/heartbeat-run-summary.ts`, `server/src/routes/agents.ts`

---

## Key Decisions

### Pre-Computed Aggregation Over Raw Event Queries

Rather than requiring API consumers and dashboards to query the full `heartbeat_run_events` log and compute statistics client-side, the server pre-computes run summaries per agent. This enables fast API responses for agent status and health-at-a-glance views.

### Summary Context for Recovery

Run summaries include context that aids process recovery — when an adapter process dies unexpectedly and leaves a run in an executing state, the summary service provides the aggregated state needed to detect and recover orphaned runs. This pairs with the orchestration layer's failure hardening to ensure no run remains permanently stuck.

### Exposed via Agent Routes

Summaries are served through the existing agent routes (`/agents` endpoints) rather than a separate summary API, keeping the API surface cohesive. Consumers querying agent status get summary data inline.

---

## Boundaries

- The **run lifecycle and state machine** are owned by `engineering/backend/heartbeat-run-orchestration`. This node covers the **aggregation and querying** layer built on top of run events.
- **Process recovery logic** (detecting and recovering orphaned runs) is part of the heartbeat orchestration service. This node provides the **summary context** that recovery depends on.
