---
title: "Database Backups"
owners: []
---

## Database Backups

Paperclip now includes built-in database backup functionality with compression and configurable retention policies.

### Features

- **Gzip compression** — Backups are gzip-compressed to reduce storage usage. Orphaned `.sql` files are cleaned up on compression failure.
- **Tiered retention** — Configurable daily, weekly, and monthly retention windows. Operators control retention periods through the Instance Settings UI.
- **CLI trigger** — The `db-backup` CLI command allows manual backup initiation.
- **UI controls** — Backup retention configuration is exposed in the Instance Settings page.

### Decisions

- Backup retention is tiered (daily/weekly/monthly) rather than a simple count-based policy, providing better long-term coverage with manageable storage.
- Configuration lives in Instance Settings (UI) rather than env-only, making it accessible to non-technical operators.
