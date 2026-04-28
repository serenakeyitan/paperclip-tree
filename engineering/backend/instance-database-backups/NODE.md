---
title: "Instance Database Backups Endpoint"
owners: [bingran-you, cryppadotta, serenakeyitan]
soft_links: [engineering/backend/NODE.md, engineering/database/NODE.md, infrastructure/backups/NODE.md]
---

# Instance Database Backups Endpoint

The backend exposes an HTTP route (`server/src/routes/instance-database-backups.ts`) that triggers on-demand database backups for the running Paperclip instance. The route reuses the shared backup library in `packages/db/src/backup-lib.ts`, which now accepts a `backupEngine` option of `"auto" | "pg_dump" | "javascript"`.

## Key Decisions

### Backup Engine Selection

Callers can pin the engine: `pg_dump` shells out to the Postgres CLI (fast, faithful), `javascript` uses an in-process cursor-based dumper (portable, no external binary needed), and `auto` picks the best available. The JS engine is the default fallback when `pg_dump` is unavailable or when transforms (excluded tables, nullified columns, migration journal handling) need to be applied row-by-row, since transforms are implemented in the JS path via `hasBackupTransforms`.

### HTTP Surface

The instance-database-backups route is mounted alongside other server routes via `server/src/routes/index.ts` and is covered by `server/src/__tests__/instance-database-backups-routes.test.ts`. It is intended as an instance-scoped operational endpoint, not a multi-tenant API.

## Related

See `infrastructure/backups/NODE.md` for the broader backup retention/scheduling story; this node covers the on-demand HTTP trigger and the engine flag.
