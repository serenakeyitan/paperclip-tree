---
title: "Route Authorization"
owners: [bingran-you, cryppadotta, serenakeyitan]
---

# Route Authorization

This node owns the **what and why** of the backend authorization model: the policy shape, where it lives, why it's factored that way, and how it's tested. For the **checklist of what to do when adding or modifying a route**, see [authenticated-route-hardening/](../authenticated-route-hardening/NODE.md) — it defers to this node for the underlying policy.

How the backend server enforces authorization on authenticated routes — who can read, mutate, or invoke operations on a given resource, and how cross-tenant access is prevented.

**Source:** `server/src/routes/` (per-route handlers), `server/src/routes/authz.ts`, `server/src/routes/workspace-command-authz.ts`, `server/src/routes/workspace-runtime-service-authz.ts`

## Key Decisions

### Dedicated Authz Helper Modules Per Concern

Authorization logic is factored into dedicated helper modules rather than inlined per route. `authz.ts` carries the shared authorization primitives; `workspace-command-authz.ts` and `workspace-runtime-service-authz.ts` hold the rules specific to workspace command invocation and workspace runtime service access. This keeps route handlers focused on request/response shape while the authz modules own the policy — so a policy change is made in one place and covered by the dedicated authz test suite.

### Cross-Tenant Access Is a First-Class Test Concern

Company-scoped isolation — all entities are scoped to a `company_id` and data must never leak across companies — is enforced at the route layer and verified with dedicated cross-tenant test suites: `agent-cross-tenant-authz-routes.test.ts`, `issue-workspace-command-authz.test.ts`, `workspace-runtime-routes-authz.test.ts`, `workspace-runtime-service-authz.test.ts`, `plugin-routes-authz.test.ts`, `agent-permissions-routes.test.ts`. Every new authenticated route that exposes a company-scoped resource is expected to have a matching authz test that attempts access from a different tenant and asserts denial.

### Private Hostname Gate at the App Level

Beyond per-route checks, the app enforces a private-hostname gate (`app-private-hostname-gate.test.ts`) that restricts certain surfaces to private/internal hostnames. This is layered on top of route authorization — a request can pass hostname gating and still be rejected by route-level authz, and vice versa.

### Idempotency for Approval Mutations

Approval routes are idempotent (`approval-routes-idempotency.test.ts`): retrying the same approval mutation must not produce duplicate state transitions or audit entries. This is important because governance mutations are logged with before/after state, and non-idempotent retries would corrupt the audit trail.

## Why This Matters

Company-scoped isolation is a core architectural guarantee — leaking data across companies would violate the governance model. Centralizing authz in dedicated modules with dedicated tests makes it possible to reason about the full authorization surface without auditing every route handler individually.
