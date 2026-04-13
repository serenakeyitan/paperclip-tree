---
type: TREE_MISS
source_id: paperclip-e392f6b1
source_commit_range: fceefe7f097543bd565309b2290f26ee19191493..b649bd454fce0c5d9aed64e6b75eb302b5d255ba
target_node: new
supersedes: null
rationale: Gzip-compressed backups with tiered daily/weekly/monthly retention and Instance Settings UI controls were added — a new operational concern not in the tree.
---
## Backup System

Paperclip supports automated database backups with compression and configurable retention policies.

### Key Decisions

- **Gzip compression.** Backups are gzip-compressed to reduce storage. Orphaned `.sql` files are cleaned up if compression fails.
- **Tiered retention.** Retention follows a daily/weekly/monthly tier model. Each tier has independently configurable retention periods.
- **UI controls.** Retention configuration is exposed in Instance Settings, allowing operators to tune backup policy without editing config files.

### Soft Links

- [Deployment](NODE.md) — backups are part of the operational deployment surface.
- [Database](../../engineering/database/NODE.md) — backups target the Postgres database.
