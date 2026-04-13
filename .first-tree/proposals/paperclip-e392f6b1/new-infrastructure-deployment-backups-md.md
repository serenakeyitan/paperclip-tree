---
type: TREE_MISS
source_id: paperclip-e392f6b1
source_commit_range: fceefe7f097543bd565309b2290f26ee19191493..b649bd454fce0c5d9aed64e6b75eb302b5d255ba
target_node: new
supersedes: null
rationale: A backup system with gzip compression and tiered daily/weekly/monthly retention with UI controls in Instance Settings was added — a new operational capability not mentioned in the deployment node.
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
