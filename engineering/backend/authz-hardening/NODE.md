---
title: "Authenticated Route Hardening"
owners: [bingran-you, cryppadotta, serenakeyitan]
soft_links: ["engineering/backend/NODE.md", "engineering/backend/shared-api-and-actor-scoped-authorization.md"]
---

# Authenticated Route Hardening

Cross-cutting conventions for hardening authenticated HTTP routes against cross-tenant access, anonymous probing, and client-supplied attribution forgery. Applies to all routes under `/server/src/routes/` that run in `authenticated` deployment mode. Source: paperclipai/paperclip#3741.

## Key Decisions

### Derive Actor Attribution from the Session, Not the Request Body

Approval resolution endpoints (`/api/approvals/:id/approve`, `/reject`, `/request-revision`) no longer accept a `decidedByUserId` field from the client. The `resolveApprovalSchema` and `requestApprovalRevisionSchema` validators drop that field entirely, and the route derives attribution from the authenticated actor (`actor.userId`). This prevents a caller from forging approval attribution by supplying another user's id in the request body. The same pattern applies to any endpoint where "who did this" must be trustworthy.

### Redact Privileged Fields for Authenticated Members Without Admin Permission

Agent detail and company agent list endpoints return `200` with `adapterConfig` and `runtimeConfig` redacted to `{}` for authenticated company members who lack the agent-admin permission, rather than `403`-ing the whole response. This lets members see that agents exist without leaking adapter/runtime secrets.

### Reject Anonymous Access Before Resource Existence Checks

Routes like `GET /api/heartbeat-runs/:id/issues`, `GET /api/skills/index`, and `GET /api/skills/:name` must return `401` for anonymous actors (`actor.type === "none"`) before performing any database lookup. This prevents anonymous callers from using existence-check timing or `404` vs `401` response differences as an enumeration oracle.

### Redact Health Metadata for Anonymous Requests in Authenticated Mode

`/health` responds with a minimal `{ status, version }` body for anonymous callers when the deployment is in `authenticated` mode, and only returns detailed metadata (dev-server status, DB probe detail) to authenticated actors. This keeps uptime probes working without leaking internal state to unauthenticated scanners.

### Centralize Workspace Runtime Authz in a Helper Module

Project and execution workspace runtime-service mutations go through `assertCanManageProjectWorkspaceRuntimeServices` and `assertCanManageExecutionWorkspaceRuntimeServices` in `server/src/routes/workspace-runtime-service-authz.ts`. New routes that touch workspace runtime config must call these helpers rather than re-implementing the checks inline.

### Private Hostname Guard Gating

`shouldEnablePrivateHostnameGuard` enables the hostname guard only when `deploymentExposure === "private"` (regardless of whether `deploymentMode` is `local_trusted` or `authenticated`). Public deployments never enable the guard. New deployment modes must explicitly opt in via this predicate.

## How to Apply

When adding a new authenticated route, mirror these patterns: never trust client-supplied actor identity, gate anonymous access explicitly at the top of the handler, redact privileged fields rather than refusing the whole response when the caller is authenticated-but-underprivileged, and route workspace-runtime mutations through the shared authz helper.
