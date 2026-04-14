---
title: "Backups"
owners: [aronprins]
---

---
title: Backups
owners:
  - bingran-you
  - cryppadotta
  - serenakeyitan
soft_links:
  - engineering/database/NODE.md
  - infrastructure/deployment/NODE.md
  - engineering/frontend/NODE.md
---

# Backups

Database backup configuration, compression, and retention for Paperclip deployments.

## Key Decisions

- **Gzip compression** — All `pg_dump` output is gzip-compressed. Chosen for broad compatibility over alternatives (bzip2, lz4).
- **Tiered retention** — Backups are retained at higher granularity for recent data and thinned over time, balancing storage cost against point-in-time recovery needs. Tiers are configurable per deployment.
- **UI-configurable** — Backup compression and retention settings are exposed as admin controls in the company settings UI, making them a per-deployment operational concern rather than hardcoded server defaults.

## Implementation Surface

- Backend: extends the existing `pg_dump`/`pg_restore` wrappers in `/packages/db`
- Frontend: company admin UI for backup policy configuration
- Storage: compressed dumps with tiered lifecycle management
