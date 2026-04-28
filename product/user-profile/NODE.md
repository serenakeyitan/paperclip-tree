---
title: "User Profile Page"
owners: [bingran-you, cryppadotta, serenakeyitan]
soft_links: [engineering/backend/NODE.md, engineering/frontend/NODE.md, product/agent-model/NODE.md, product/governance/NODE.md]
---

# User Profile Page

Each company member now has a dedicated profile page at `/u/:userSlug` (within a company board) backed by `GET /companies/:companyId/users/:userSlug/profile`. The page surfaces a member's recent activity, cost, and assignment footprint inside the company.

## Key Decisions

### Slug-Addressed, Company-Scoped
Profiles are addressed by user *slug*, not user id, and are always scoped to a company. Access is gated by `assertCompanyAccess`, so a user only sees profiles of members of companies they belong to. There is no instance-wide profile view.

### Three Fixed Time Windows
The API computes stats for three windows — `last7`, `last30`, and `all` — rather than accepting an arbitrary range. Each window returns: touched issues, created issues, completed issues, currently-assigned open issues, comment count, activity count, cost cents, input/cached/output tokens, and a cost-event count. A `daily` series provides per-day points for charting.

### Identity Comes From Membership, Not Auth Users Alone
The identity block is assembled from `company_memberships` joined with `auth_users`, so the profile reflects the user's membership status and role *within this company* (including `archived` members). Display name, email, and avatar fall back through the auth user.

### Agent and Provider Usage Are First-Class
The response includes `agentUsage` and `providerUsage` rollups derived from `cost_events`, so a profile shows which agents and which model providers a user has been driving — useful for spend attribution and for the archival flow's reassignment picker.

## Related

- Identity and membership status come from the access model.
- Cost rollups read from the same `cost_events` table the dashboard uses.
