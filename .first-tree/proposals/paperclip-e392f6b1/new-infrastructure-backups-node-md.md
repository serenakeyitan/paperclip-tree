---
type: TREE_MISS
source_id: paperclip-e392f6b1
source_commit_range: db4e1465517f6e96876dda85488d4ab7210412a1..5d1ed71779df5622d9fd99ad28816b2da4bdee31
target_node: new
rationale: PR #3015 introduces a new backups domain with gzip compression, tiered retention policies, and UI controls — no existing node covers backup configuration or retention strategy.
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
