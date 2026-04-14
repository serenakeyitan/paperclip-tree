---
type: TREE_MISS
source_id: paperclip-e392f6b1
source_commit_range: db4e1465517f6e96876dda85488d4ab7210412a1..5d1ed71779df5622d9fd99ad28816b2da4bdee31
target_node: new
rationale: PR introduces gzip compression, tiered retention policies, and UI controls for backups — a new operational domain with no dedicated tree node (only a one-line mention in engineering/database/NODE.md).
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
