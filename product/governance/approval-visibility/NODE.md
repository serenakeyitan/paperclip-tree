---
title: "Approval Visibility in Mine & Badge"
owners: [bingran-you, cryppadotta, serenakeyitan]
soft_links: [engineering/frontend/inbox-list/NODE.md, product/governance/NODE.md, product/governance/issue-approvals/NODE.md]
---

# Approval Visibility in Mine & Badge

Defines which approvals show up in a user's personal inbox surfaces — the `mine` tab and the inbox badge count — versus the company-wide `recent` and `unread` tabs.

## Key Decisions

### Actionable Approvals Are Always Visible in Mine

Any approval whose status is in `ACTIONABLE_APPROVAL_STATUSES` (e.g. `pending`, `revision_requested`) is shown in the `mine` tab and counted in the inbox badge, even if the current user neither requested nor decided it. This explicitly covers agent-requested approvals (where `requestedByUserId` is `null`) so that work waiting on a human cannot get lost just because no specific user is on the request. The shared predicate is `isApprovalVisibleInMine` in `ui/src/lib/inbox.ts`.

### Resolved Approvals Are Personal

For non-actionable statuses (approved, rejected, etc.), the previous rule still applies: an approval only appears in `mine` if the current user requested it or decided it. This keeps historical noise out of other users' personal views while still giving the requester/decider a record.

### Recent and Unread Stay Company-Wide

The `recent` and `unread` tabs continue to show approvals across the whole company regardless of requester/decider. Only `mine` and the badge apply the visibility predicate above.
