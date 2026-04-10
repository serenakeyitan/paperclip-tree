---
title: "Company Model"
owners: [bingran-you, cryppadotta, serenakeyitan]
soft_links: ["product/governance/NODE.md", "product/agent-model/NODE.md", "engineering/NODE.md"]
---

# Company Model

How companies are represented, scoped, and made portable in Paperclip.

## Key Decisions

### Company as First-Order Object

A Company is the top-level organizational boundary. One Paperclip instance runs multiple Companies. Every business entity (agent, task, goal, project, cost event) belongs to exactly one company. Company-scoping is enforced at every entity fetch and mutation.

**Rationale:** The "company" abstraction maps directly to the real-world concept of an autonomous business. It provides natural isolation boundaries for data, budgets, and governance without needing complex multi-tenant permission schemes.

### No Standalone Goal Field

A Company's direction is defined by its set of **Initiatives** (the highest-level planning construct), not a single "goal" string field. The company goal from PRODUCT.md becomes the first/primary Initiative.

**Rationale:** Real companies pursue multiple strategic objectives simultaneously. A single goal field would be too rigid; initiatives provide the needed flexibility while still anchoring all work to company direction.

### Single-Tenant Deployment, Multi-Company Data Model

Paperclip is not a SaaS. One deployment = one human operator's companies. Multi-company support is about one person running multiple AI businesses, not about sharing a platform across unrelated users.

**Rationale:** Single-tenancy eliminates an entire class of isolation, billing, and trust-boundary problems. The multi-company model lets power users run diverse AI businesses from one control plane.

### Company Lifecycle

Companies have three statuses: `active`, `paused`, `archived`. Paused companies stop all heartbeats. Archived companies are read-only historical records.

### Company Portability

V1 supports import/export using a portable package contract:

- **Markdown-first** format rooted at `COMPANY.md`, vendor-neutral by default
- **Template export** (default): structure only -- agent definitions, org chart, adapter configs, role descriptions, optional seed tasks
- **Snapshot export**: full state including current tasks, progress, and agent status
- **`.paperclip.yaml` sidecar** adds Paperclip-specific fidelity (routines, workspace metadata)
- Export never includes secret values; env inputs become portable declarations
- Import supports creating new companies or importing into existing ones, with collision strategies (`rename`, `skip`, `replace`) and dry-run preview

**Rationale:** Exportable company configs enable sharing templates ("pre-built marketing agency"), version-controlling org structure, and duplicating/forking companies. The markdown-first approach keeps packages readable and vendor-neutral.

## Decision Records

- [company-is-the-top-level-boundary.md](company-is-the-top-level-boundary.md) — Why Company is the primary isolation, governance, and work boundary in Paperclip.

## Open Questions

- Company-level settings and configuration surface (beyond what exists today)
- External revenue/expense tracking integration points (deferred to plugin)
