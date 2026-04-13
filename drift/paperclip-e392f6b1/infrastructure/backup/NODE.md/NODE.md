---
title: "Backup and Retention"
owners: []
---

## Overview

Paperclip supports automated database backups with compression and configurable retention policies, managed through the Instance Settings UI.

## Key Decisions

- **Gzip compression** — Backups are gzip-compressed to reduce storage costs. Orphaned `.sql` files from failed compressions are cleaned up automatically.
- **Tiered retention** — Retention follows a daily/weekly/monthly tier model. Administrators configure how many backups to keep at each tier via the Instance Settings UI.
- **UI-driven configuration** — Retention settings are exposed in Instance Settings rather than requiring env vars or config file edits, making them accessible to non-technical operators.
- **Failure resilience** — Compression failures are handled gracefully with cleanup of partial artifacts and accurate startup logs.
