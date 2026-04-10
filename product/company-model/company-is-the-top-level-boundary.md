---
title: "Company Is The Top-Level Organizational Boundary"
owners: [bingran-you, cryppadotta, serenakeyitan]
---

# Company Is The Top-Level Organizational Boundary

Paperclip is built around the idea that an autonomous company is the primary
unit of organization, not a team, workspace, or chat room.

## Decision

Treat the Company as the first-order boundary for data, governance, budgeting,
and work organization. Every business entity belongs to exactly one company,
while one deployment may host multiple companies for the same operator.

## Why

This matches the product's core metaphor, creates a natural isolation boundary,
and avoids the extra trust and billing complexity that a multi-tenant SaaS
model would introduce in V1.

## Implications

- Cross-company data access should be treated as an exception or bug, not a
  routine workflow.
- Budget rollups, approvals, org structure, tasks, and costs all anchor at the
  company level before going deeper.
- Import/export and portability features should preserve the company as the
  stable package boundary.
- Deployment-level multi-company support exists for one operator running
  multiple businesses, not for unrelated operators sharing an instance.

## Related Domains

- [product governance](../governance/NODE.md)
- [product agent model](../agent-model/NODE.md)
- [engineering database](../../engineering/database/NODE.md)
