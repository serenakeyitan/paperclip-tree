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

Database backup infrastructure with compression, retention management, and admin-facing configuration UI.

## Key Decisions

- **Gzip compression** — All database dumps are gzip-compressed before storage for reduced footprint. Chosen for broad compatibility over alternatives like zstd.
- **Tiered retention** — Recent backups kept at high granularity; older backups thinned over time. Balances storage cost against point-in-time recovery needs.
- **UI-configurable policies** — Backup compression and retention settings are exposed through the admin UI, not hardcoded. Company administrators can tune policies per deployment.
- **pg_dump/pg_restore foundation** — Built on the existing DB package wrappers for data portability.

## Leaf Nodes

None yet.
