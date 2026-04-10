---
title: "Shared API And Actor-Scoped Authorization"
owners: [bingran-you, cryppadotta, serenakeyitan]
---

# Shared API And Actor-Scoped Authorization

Paperclip does not maintain one HTTP surface for humans and another for agents.
The backend uses a shared API surface and makes authorization a property of the
resolved actor on each request.

## Decision

Resolve request actors centrally in middleware, then apply company access and
role-specific checks at the route layer. Board sessions, board API keys, agent
API keys, and local agent JWTs all flow into the same request actor model, and
routes decide what the resolved actor may do.

## Why

This keeps the control plane easier to reason about than separate "board API"
and "agent API" products. It also preserves one canonical way to express
company access, instance-admin overrides, agent scoping, and deployment-mode
guards across the server.

## Implications

- New routes should consume `req.actor` plus shared auth helpers instead of
  inventing ad hoc credential parsing or role checks.
- Company scoping is enforced before feature-specific permissions, so agent and
  human requests stay inside the same company boundary model.
- Deployment-specific protections such as trusted browser mutation checks and
  private-hostname gating layer on top of the same actor model rather than
  replacing it.
- Product decisions about who may invoke a route should usually become route
  assertions or access-service checks, not a separate API namespace.

## Related Domains

- [database](../database/NODE.md)
- [product governance](../../product/governance/NODE.md)
- [product company model](../../product/company-model/NODE.md)
