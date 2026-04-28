---
title: "Member Archival"
owners: [bingran-you, cryppadotta, serenakeyitan]
soft_links: [engineering/backend/NODE.md, product/governance/NODE.md, product/task-system/NODE.md]
---

# Member Archival

Company memberships now support an **archived** status in addition to `pending`, `active`, and `suspended`. Archival is the supported way to remove a human from a company while preserving historical attribution on issues, comments, and activity.

## Key Decisions

### Archived Is a Terminal Status, Not a Delete
Membership rows are never deleted. Archiving flips `status` to `archived`, revokes all `principal_permission_grants` for that principal in the company, and clears the membership role where appropriate. History (authored comments, activity log entries, cost events) stays intact and continues to attribute to the original user.

### Editable Statuses Exclude `archived`
The `updateCompanyMemberSchema` validator restricts editable statuses to `pending | active | suspended`. Moving a member into or out of `archived` goes through the dedicated archive endpoint, not the generic status update path. This keeps the archival side-effects (grant revocation, issue reassignment) atomic and auditable.

### Open Issues Must Be Reassigned
Archiving a member requires reassigning any open issues currently assigned to them. The archive endpoint accepts a `reassignment` payload with either `assigneeAgentId` or `assigneeUserId` and returns the count of reassigned issues. The server refuses to archive the last active owner of a company — the `canArchive` flag and `reason` string on each member record surface this constraint to the UI.

### Removal Metadata on Member Records
`CompanyMemberRecord` now carries an optional `removal: { canArchive, reason }` block so the access UI can disable the archive action and explain why (e.g. 'last active owner') without a second round-trip.

## Related

- Permission grants are revoked via `principal_permission_grants` cleanup; see governance node.
- Issue reassignment touches the task system's assignee model.
