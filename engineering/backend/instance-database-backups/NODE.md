---
title: "Instance Database Backups API"
owners: [bingran-you, cryppadotta, serenakeyitan]
soft_links: ["engineering/database/NODE.md", "infrastructure/backups/NODE.md"]
---

# Instance Database Backups API

The backend exposes an authenticated instance-scoped backup trigger at `POST /api/instance/database-backups`. The route is wired from `server/src/routes/instance-database-backups.ts` through `server/src/app.ts`, and the server fulfills the request by calling `runDatabaseBackup` from `packages/db/src/backup-lib.ts`.

## Key Decisions

### Instance-admin access only

Manual backups are treated as an instance-level operational control, not a company-scoped action. The route uses `assertInstanceAdmin`, so it allows board users with instance-admin access plus local implicit board access in `local_trusted` mode, while rejecting ordinary board members and all agent callers.

### Manual and scheduled backups share one runner

The HTTP route does not implement a separate backup code path. `server/src/index.ts` routes both manual requests and scheduled backups through the same `runServerDatabaseBackup` function, which reads the current retention policy from instance settings at execution time and returns backup metadata including file path, retention snapshot, and timing information. A single in-flight guard prevents overlap: scheduled runs are skipped when a backup is already running, while manual requests return HTTP 409.

### Auto engine prefers `pg_dump` but falls back to JavaScript

`runDatabaseBackup` accepts `backupEngine: "auto" | "pg_dump" | "javascript"`. In `auto` mode it first takes the `pg_dump` path when no transforms are requested, because the CLI dump is the fast path for plain backups. If that attempt fails, `auto` reopens the database connection and falls back to the in-process JavaScript dumper instead of failing immediately. Explicit `pg_dump` mode does not fall back.

### Backup transforms force the JavaScript dumper

The shared backup library treats `includeMigrationJournal`, `excludeTables`, and `nullifyColumns` as transform flags. When any of them are present, `hasBackupTransforms()` disables the `pg_dump` fast path so the backup can be emitted row-by-row with transform-aware SQL output. In that mode the dumper streams rows through a cursor (`BACKUP_DATA_CURSOR_ROWS = 100`) and can nullify selected columns or include Drizzle migration metadata in the generated SQL.

## Boundaries

- Retention policy shape and instance-settings UI live under `infrastructure/backups/`.
- This node covers the backend API surface and execution-path decisions, not restore tooling or storage layout.
