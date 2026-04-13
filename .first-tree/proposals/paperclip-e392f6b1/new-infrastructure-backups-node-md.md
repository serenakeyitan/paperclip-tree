---
type: TREE_MISS
source_id: paperclip-e392f6b1
source_commit_range: db4e1465517f6e96876dda85488d4ab7210412a1..5d1ed71779df5622d9fd99ad28816b2da4bdee31
target_node: new
rationale: PR introduces gzip compression, tiered retention policies, and UI controls for backups — a full backup configuration subsystem not covered by the existing one-line mention in engineering/database/NODE.md.
---
# Backups & Retention

Backup configuration, compression, and retention policies for Paperclip data.

## Backup Compression

Backups support gzip compression to reduce storage footprint. Compression is applied to `pg_dump` output before writing to the configured storage backend.

## Tiered Retention

Backups follow a tiered retention policy that automatically manages backup lifecycle:

- Recent backups are retained at full granularity.
- Older backups are thinned according to configurable retention tiers (e.g., daily for 7 days, weekly for 4 weeks, monthly for 12 months).
- Retention policies are configurable per deployment.

## UI Controls

The frontend provides UI controls for managing backup configuration — compression settings, retention tier definitions, and backup scheduling. This allows operators to configure backup behavior without editing config files directly.

## Key Decisions

- **Gzip over alternative compression.** Gzip was chosen for broad compatibility and acceptable compression ratios on SQL dump output.
- **Tiered retention is built-in, not delegated.** Retention logic runs within Paperclip rather than relying on external storage lifecycle rules, keeping the system self-contained for single-node deployments.
