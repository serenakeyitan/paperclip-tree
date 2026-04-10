---
title: "Company-Scoped Isolation"
owners: [bingran-you, cryppadotta, serenakeyitan]
---

# Company-Scoped Isolation

Paperclip runs multiple companies inside one control-plane deployment, but it
does not treat those companies as soft organizational labels. Company identity
is a load-bearing system boundary.

## Decision

Use a single PostgreSQL schema and enforce isolation by carrying `company_id`
through company-scoped tables, actor resolution, and service-layer queries.
Paperclip does not use a separate database per company, and it does not rely
on database row-level security as the primary isolation mechanism.

## Why

This keeps the platform operationally simple for local-first and small-team
deployments while preserving the product model that one Paperclip instance can
host multiple independent companies. It also keeps cross-company operations,
instance admin workflows, and shared infrastructure manageable without adding
tenant-per-database complexity.

## Implications

- New domain tables that belong to a company should carry `company_id` and be
  indexed in company-aware access patterns.
- Authentication and actor resolution must always produce company context for
  agent and board actions before service logic runs.
- Isolation bugs are application bugs, not something the database will
  automatically rescue. Service code has to treat company scoping as a first
  principle.
- Features that introduce shared infrastructure, such as execution workspaces,
  costs, or approvals, still need an explicit answer for how company context is
  attached and enforced.

## Related Domains

- [company-model](../../product/company-model/NODE.md)
- [backend](../backend/NODE.md)
