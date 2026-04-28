---
title: "Pooled vs Direct Database Connections"
owners: [bingran-you, cryppadotta, serenakeyitan]
soft_links: [engineering/database/NODE.md, infrastructure/deployment/NODE.md]
---

Paperclip's database connection policy distinguishes runtime queries from migration/startup work. The application runtime should use a **direct** PostgreSQL connection (e.g. Supabase port 5432) unless the database client has explicit prepared-statement configuration for the chosen pooling mode. Pooled URLs (port 6543) are valid only when the client is configured to disable prepared statements; otherwise pooled-mode pgbouncer breaks `postgres-js`.

For hosted deployments that genuinely need a pooled runtime URL, set `DATABASE_MIGRATION_URL` to the **direct** connection string. Paperclip uses `DATABASE_MIGRATION_URL` for startup schema checks, core migrations, and plugin namespace migrations, while `DATABASE_URL` continues to drive runtime queries. Migrations must always run against a direct connection so schema operations and prepared statements behave correctly.

The canonical migration command is `pnpm db:migrate` (not `drizzle-kit push` for production paths). `drizzle-kit push` remains a local-development convenience.

See `doc/DATABASE.md` for the canonical configuration examples.
