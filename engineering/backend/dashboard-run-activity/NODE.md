---
title: "Dashboard Run Activity Aggregation"
owners: [bingran-you, cryppadotta, serenakeyitan]
soft_links: [engineering/backend/NODE.md, engineering/backend/heartbeat-run-orchestration/NODE.md, engineering/frontend/NODE.md, engineering/shared/NODE.md]
---

The dashboard summary endpoint includes a server-aggregated `runActivity` field: a 14-day window of heartbeat run counts bucketed by UTC date, with `succeeded`, `failed`, `other`, and `total` per day. The shape is defined by the shared `DashboardRunActivityDay` type in `packages/shared/src/types/dashboard.ts` and consumed by the dashboard UI's `RunActivityChart`.

## Key Decisions

### Server-Side Aggregation Over Client-Side Run Fetch

Previously the dashboard fetched up to 100 recent heartbeat runs and aggregated them in the browser. The dashboard service (`server/src/services/dashboard.ts`) now performs the aggregation directly against `heartbeatRuns` for the last 14 UTC days, returning pre-bucketed counts. This avoids transferring per-run rows that the chart never rendered, keeps the chart accurate when run volume exceeds the old 100-row cap, and removes a redundant `heartbeatsApi.list` call from `ui/src/pages/Dashboard.tsx`.

### UTC Day Bucketing

Days are keyed by `YYYY-MM-DD` derived from UTC (`Date.UTC(...)`), and `monthStart` for spend was also moved to UTC for consistency. All clients must interpret the date keys as UTC days; mixing local-time bucketing on the UI side will misalign the chart.

### Backwards-Compatible Chart Component

`RunActivityChart` accepts either pre-aggregated `activity: DashboardRunActivityDay[]` (the new dashboard path) or raw `runs: HeartbeatRun[]` (legacy/other surfaces). The component's `aggregateRuns` helper mirrors the server bucketing so both inputs produce identical visuals.
