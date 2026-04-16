---
title: "Route Authorization & Cross-Tenant Isolation"
owners: [bingran-you, cryppadotta, serenakeyitan]
---

# Route Authorization & Cross-Tenant Isolation

Authenticated Express routes in the Paperclip server enforce company-scoped isolation through dedicated authorization helpers. Every mutating or read endpoint that touches a company-scoped entity (agents, approvals, plugins, execution workspaces, issues, activity, projects) must verify that the requesting principal belongs to the owning company before resolving the resource. When a company-scoped resource exists but belongs to a different tenant, the route returns an authorization error (403) rather than the resource; when the resource does not exist at all, the route still returns 404. This is the invariant exercised by `agent-cross-tenant-authz-routes.test.ts` and `workspace-runtime-routes-authz.test.ts`. This node should not be read as claiming the stronger anti-enumeration guarantee of making missing and foreign resources indistinguishable — the current implementation does not provide that.

## Key Decisions

### Dedicated authz helper modules per concern

Authorization logic for workspace commands and workspace runtime services lives in standalone modules (`server/src/routes/workspace-command-authz.ts`, `server/src/routes/workspace-runtime-service-authz.ts`, `server/src/routes/authz.ts`) rather than being inlined into each route handler. This keeps the route handlers focused on request/response shaping while the authz modules own the company-scope checks, principal resolution, and consistent error responses. New routes that touch the same resources should reuse these helpers instead of re-implementing the checks.

### Idempotency for approval mutations

Approval routes are idempotent: repeating the same approval action with the same payload does not produce duplicate side effects or audit entries. This is a hard requirement because approval flows are reached from multiple surfaces (UI, CLI, webhooks) where retries are common. The idempotency contract is enforced and exercised by `approval-routes-idempotency.test.ts`.

### Private hostname gate at the app layer

The Express app applies a private-hostname gate (`app-private-hostname-gate.test.ts`) so that internal endpoints cannot be reached from public hostnames even if a route is otherwise authenticated. This is a defense-in-depth boundary on top of per-route authz — routes intended for internal callers should rely on this gate rather than re-implementing hostname checks.

## Test Coverage Convention

Every route family that exposes company-scoped data has a corresponding `*-authz.test.ts` or cross-tenant test in `server/src/__tests__/` (agent-cross-tenant-authz-routes, agent-permissions-routes, plugin-routes-authz, workspace-runtime-routes-authz, workspace-runtime-service-authz, issue-workspace-command-authz, cli-auth-routes). When adding a new company-scoped route, an authz test that asserts both the positive (same-company access succeeds) and negative (cross-company access is rejected) cases is required.
