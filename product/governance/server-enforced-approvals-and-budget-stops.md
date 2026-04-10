---
title: "Server-Enforced Approvals And Budget Stops"
owners: [bingran-you, cryppadotta, serenakeyitan]
---

# Server-Enforced Approvals And Budget Stops

Paperclip's governance model only works if approval gates and budget ceilings
remain control-plane invariants instead of runtime conventions.

## Decision

Approval resolution, budget policy enforcement, pause/resume state changes, and
their audit consequences are enforced in server services. Agents and adapters
can request actions and report usage, but they do not get to finalize approval
outcomes or decide whether governance rules apply.

## Why

This preserves board authority even when many different runtimes are executing
work. It also ensures that the same governance rules hold across local
development, authenticated deployments, and future runtime integrations.

## Implications

- Approval types such as hiring remain canonical server objects whose state
  transitions decide whether downstream mutations are applied.
- Budget hard stops pause the affected scope in the platform state model and
  can cancel work, rather than relying on runtimes to self-regulate.
- Auditability depends on the server being the mutation authority for these
  flows, because the activity log and approval records are part of the product
  contract.
- New features that spend money, create authority, or change execution rights
  must integrate with the server governance path instead of bypassing it.

## Related Domains

- [engineering backend](../../engineering/backend/NODE.md)
- [company model](../company-model/NODE.md)
- [agent model](../agent-model/NODE.md)
