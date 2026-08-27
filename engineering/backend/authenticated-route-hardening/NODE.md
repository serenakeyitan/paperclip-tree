---
title: "Authenticated Route Hardening"
owners: [bingran-you, cryppadotta, serenakeyitan]
---

# Authenticated Route Hardening

This node is the **practical checklist for adding or modifying authenticated routes** — the recurring patterns (per-route tenant check, private-hostname gate, idempotency, co-located authz test) contributors should apply when touching the routes layer. For the **underlying authorization model** (helper-module factoring, cross-tenant policy, why it's centralized), see [route-authorization/](../route-authorization/NODE.md); that node owns the *what and why*, this node owns the *how-to*.

Paperclip's backend applies a consistent hardening pattern across authenticated HTTP routes. The pattern enforces tenant isolation, private-hostname gating, and idempotency on state-changing endpoints, with dedicated test suites per route family to prevent regressions.

## Key Decisions

### Cross-Tenant Authorization Is Per-Route, Not Middleware-Only

Each authenticated route family (agents, approvals, activity, issues, plugins, execution workspaces, projects, access) re-validates that the caller's company scope matches the target entity's `company_id`. Relying on middleware alone was judged insufficient because routes frequently load entities by ID before scope checks — the per-route guard ensures no handler can return or mutate another tenant's data even if middleware assumptions shift.

### Private Hostname Gate at the App Layer

A private-hostname gate lives in `server/src/app.ts` and short-circuits requests that should never be served from public hostnames. This is enforced at the app layer (not per-route) so that new routes inherit the protection by default.

### Approval Route Idempotency

Approval mutations are idempotent: replaying the same approval action must not double-apply side effects or corrupt the audit trail. This is validated by a dedicated `approval-routes-idempotency` test suite and the shared `validators/approval.ts` schema, which is the single source of truth for approval payload shape across server and clients.

### Authz Test Coverage as a First-Class Artifact

Every authenticated route family has a matching `*-authz.test.ts` file (agent-cross-tenant, agent-permissions, plugin-routes, workspace-runtime-routes, workspace-runtime-service, issue-workspace-command, cli-auth). New authenticated routes are expected to ship with their authz test file; the pattern is enforced by convention, not tooling.

## How To Apply

When adding or modifying an authenticated route:

- Validate `company_id` scope inside the handler, even if middleware already ran.
- If the route mutates state, ensure replay safety and add an idempotency test.
- Reuse the shared validator in `packages/shared/src/validators/` rather than redefining payload shape.
- Add a co-located `*-authz.test.ts` covering cross-tenant access, missing permissions, and (where relevant) the private-hostname gate.
