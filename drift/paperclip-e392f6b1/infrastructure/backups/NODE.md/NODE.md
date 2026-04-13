---
title: "Backups"
owners: [@bingran-you, @serenakeyitan, @bingran-you, @serenakeyitan]
---

Backup strategy, compression, retention policies, and configuration UI for Paperclip data.

## Overview

Paperclip provides built-in backup capabilities wrapping `pg_dump`/`pg_restore` (see [engineering/database](../../engineering/database/NODE.md)). This domain covers the operational layer on top: how backups are compressed, how long they are retained, and how operators configure these settings.

## Compression

Backups use **gzip compression** to reduce storage footprint. Gzip was chosen for broad compatibility and acceptable compression ratio for database dumps.

## Tiered Retention

Backups follow a **tiered retention policy** — recent backups are kept at higher granularity, while older backups are thinned out over time. The specific retention tiers are configurable. A tiered approach balances storage cost against the need for recent point-in-time recovery while keeping longer-term snapshots for disaster recovery.

## UI Configuration

Backup configuration (compression settings, retention tiers) is exposed through the Paperclip frontend UI, allowing company administrators to manage backup policies without direct server access. Backup policies are a per-deployment operational concern, so they are exposed as admin-configurable settings rather than hardcoded defaults.
