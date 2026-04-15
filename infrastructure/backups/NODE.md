---
title: "Backups"
owners: [bingran-you, cryppadotta, serenakeyitan, aronprins]
---

# Backups

Backup infrastructure for Paperclip data, covering automated database snapshots, compression, retention policies, and user-facing configuration.

## Compression

Backups use **gzip compression** to reduce storage footprint. This applies to all backup artifacts produced by the system.

## Tiered Retention

Retention follows a three-tier policy with preset options validated at the schema level:

- **Daily tier:** keep all backups from the last N days. Presets: `3`, `7`, or `14` days.
- **Weekly tier:** keep the newest backup per calendar week for N weeks. Presets: `1`, `2`, or `4` weeks.
- **Monthly tier:** keep the newest backup per calendar month for N months. Presets: `1`, `3`, or `6` months.

Defaults: daily for 7 days, weekly for 4 weeks, monthly for 1 month. Backups beyond all retention tiers are pruned automatically.

## UI Controls

Retention preset configuration is exposed through `InstanceGeneralSettings` in the settings UI. The UI allows company admins to select from the validated preset values for each tier. No backup status dashboard or manual trigger UI has been shipped yet.

## Key Decisions

- **Gzip over raw dumps.** Storage efficiency matters for self-hosted deployments; gzip is universally supported and adds minimal CPU overhead.
- **Tiered retention over flat TTL.** A single retention window either keeps too many old backups or loses recent granularity. Tiered retention balances storage cost with recovery flexibility.
- **User-configurable via UI.** Backup policy is an operational concern that company admins should control without touching config files or CLI.
