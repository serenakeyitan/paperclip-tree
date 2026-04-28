---
title: "Auth, Memberships, and Permission Grants"
owners: [bingran-you, cryppadotta, serenakeyitan]
soft_links: [product/company-model/NODE.md, product/governance/NODE.md, product/governance/hire-approval-default/NODE.md, product/governance/issue-approvals/NODE.md]
---

Paperclip has moved past the original `local_trusted` implicit-board posture. The current product surface includes authenticated mode, company memberships, instance roles, and explicit permission grants — enough to support multi-user company-scoped governance without enterprise-grade RBAC.

The deliberate boundary: fine-grained enterprise governance (org-wide policy engines, SSO-driven role mapping, attribute-based access control) remains secondary to the core company control plane. Paperclip ships coarse, company-scoped permissions first; richer governance is layered on later if demand justifies it. The non-goal in `doc/PRODUCT.md` is updated accordingly — "do not build enterprise-grade RBAC first" still holds, but no longer means "no auth model exists."

This matters for design decisions in adjacent domains: hire approvals, issue approvals, and budget enforcement all assume an identity model that can attribute actions to a real user or agent. Anything that mutates company state should route through the auth/permissions surface rather than reintroducing an implicit-board shortcut.

See `doc/SPEC-implementation.md` §V1 Decisions and the schema additions for `pause_reason`, `paused_at`, `issue_prefix`, `budget_monthly_cents`, etc.
