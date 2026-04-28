---
title: "API Route Authorization Boundaries"
owners: [bingran-you, cryppadotta, serenakeyitan]
soft_links: [adapters/NODE.md, engineering/backend/NODE.md, product/governance/NODE.md, product/governance/issue-approvals/NODE.md, product/task-system/NODE.md]
---

Paperclip's HTTP API enforces a layered authorization model on top of company-scoped isolation. Routes fall into four tiers, and PR #4122 codified them after an audit found several routes that conflated tiers.

**Instance-admin only.** Mutating adapter-management routes (`POST /api/adapters/install`, reload, disable/enable) install or toggle server-side adapter code for the entire Paperclip instance, so they require `assertInstanceAdmin`. Read-only `GET /api/adapters` stays at board-org access because ordinary board members need the adapter inventory to create or edit company agents.

**Board-only mutations.** Agent budget updates (`PATCH /api/agents/:id/budgets`) are board-only via `assertBoard` — agents may no longer raise their own budgets. Direct agent creation (`POST /api/companies/:companyId/agents`) checks `company.requireBoardApprovalForNewAgents`; when true, the route 409s and clients must instead `POST /api/companies/:companyId/agent-hires` to create a pending hire approval. Agent creation also rejects attempts to mutate host workspace commands or instructions config from non-privileged actors.

**Company-portability gates.** Export and import routes use `assertCanManagePortability(req, companyId, kind)` rather than plain `assertCompanyAccess`. CEO-safe export preview routes reject non-CEO agents.

**Issue mutation ownership.** Issue routes assert checkout ownership before allowing comment/attachment/relation mutations from agents, gated by the new `tasks:manage_active_checkouts` permission key (added to `PERMISSION_KEYS` in `packages/shared/src/constants.ts`). Peer agents in the same company cannot mutate an issue another agent currently has checked out unless they hold this permission.

**Invite resolution SSRF hardening.** `POST /api/access/invites/resolve` now resolves the target hostname via DNS (3s timeout), rejects private/loopback/link-local IPs, pins the resolved address for the HTTP HEAD probe, and preserves the original Host header and TLS SNI. This prevents the invite-resolution probe from being used as an SSRF primitive against internal services.
