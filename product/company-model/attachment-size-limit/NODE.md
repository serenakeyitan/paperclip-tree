---
title: "Company Attachment Size Limit"
owners: [bingran-you, cryppadotta, serenakeyitan]
soft_links: [engineering/database/NODE.md, product/company-model/NODE.md, product/governance/NODE.md]
---

# Company Attachment Size Limit

Each company carries an `attachmentMaxBytes` setting that caps the size of file attachments uploaded into that company's workspaces. The value is stored on the `companies` row (`attachment_max_bytes` column) and is enforced server-side when attachments are accepted.

## Key Decisions

### Per-Company, Not Global

The attachment ceiling is a per-company setting rather than a server-wide constant. Different companies can run with different limits without redeploying or coordinating across tenants. This keeps with the broader Paperclip model that all governance-style limits (budgets, approvals, etc.) are scoped to `company_id`.

### Default 10 MiB, Hard Ceiling 1 GiB

`DEFAULT_COMPANY_ATTACHMENT_MAX_BYTES` is `10 * 1024 * 1024` (10 MiB) — applied both as the schema default for new companies and as the seed value for existing rows during migration `0073_shiny_salo`. `MAX_COMPANY_ATTACHMENT_MAX_BYTES` is `1024 * 1024 * 1024` (1 GiB), which validators use as the upper bound when a company admin raises the limit. The 10 MiB default keeps attachments cheap and predictable; the 1 GiB ceiling prevents pathological values that would blow up storage or transfer.

### Surfaced Through Company Portability

`attachmentMaxBytes` is part of the company portability shape (import/export), so moving a company between deployments preserves its configured limit. Import preview includes the field even when null (meaning "keep destination default").
