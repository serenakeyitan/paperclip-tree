---
title: "Member Archival"
owners: [bingran-you, cryppadotta, serenakeyitan]
---

# Member Archival

Removing a human from a company should revoke their access without erasing the
history that explains past work, approvals, and ownership.

## Decision

Human company memberships use a dedicated archival flow that moves the
membership into a terminal `archived` state instead of deleting the record or
treating archival as a generic editable status. Archiving clears the member's
direct permission grants, prevents removal of the last active owner, and can
reassign the member's open issues to another active company principal.

## Why

Governance needs member removal to preserve auditability and historical task
context. Deleting memberships would break the traceability of who previously
owned work or held authority inside the company. Making archival a dedicated
flow, rather than just another status update, ensures the owner-removal guard
and work-reassignment rules always run when access is revoked.

## Implications

- Human membership lifecycle includes a terminal `archived` state that keeps
  authored activity and past assignments intelligible while removing active
  access.
- Member-management surfaces should present archival as a separate action from
  routine role or status edits.
- Open issues assigned to an archived human must be reassigned as part of the
  archival operation instead of silently remaining with an inactive assignee.
- Company governance treats the last active owner as a protected role; archival
  cannot bypass that invariant.
- Agent lifecycle remains separate from human membership archival even when work
  is reassigned to an agent.

## Related Domains

- [product company model](../company-model/NODE.md)
- [product task system](../task-system/NODE.md)
- [engineering backend](../../engineering/backend/NODE.md)
