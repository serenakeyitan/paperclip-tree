---
type: TREE_MISS
source_id: paperclip-e392f6b1
source_commit_range: 5e65bb2b92ae765815b6816cef60c25cdda837ca..7f893ac4ec9f700efaf902be8a57ce510c1c7092
target_node: new
rationale: The PR introduces a full backup subsystem with gzip compression, tiered retention policies, and UI configuration — this is a new infrastructure concern with its own configuration surface, not just a DB utility.
---
# Backups

Backup infrastructure for Paperclip data, covering automated database snapshots, compression, retention policies, and user-facing configuration.

## Compression

Backups use **gzip compression** to reduce storage footprint. This applies to all backup artifacts produced by the system.

## Tiered Retention

Retention follows a tiered policy — recent backups are kept at higher granularity, older backups are thinned out over time. The specific retention tiers (e.g., daily for 7 days, weekly for 4 weeks, monthly for 12 months) are configurable.

## UI Controls

Backup configuration is exposed through the frontend UI, allowing company administrators to view backup status, trigger manual backups, and adjust retention settings. This is a governance-adjacent feature: backup policy is part of operational control for AI companies running on Paperclip.

## Key Decisions

- **Gzip over raw dumps.** Storage efficiency matters for self-hosted deployments; gzip is universally supported and adds minimal CPU overhead.
- **Tiered retention over flat TTL.** A single retention window either keeps too many old backups or loses recent granularity. Tiered retention balances storage cost with recovery flexibility.
- **User-configurable via UI.** Backup policy is an operational concern that company admins should control without touching config files or CLI.
