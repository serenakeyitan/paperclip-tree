---
title: "Plugin Database Namespaces"
owners: [bingran-you, cryppadotta, serenakeyitan]
soft_links: [engineering/database/NODE.md, plugins/examples/NODE.md, plugins/runtime/NODE.md, plugins/sdk/NODE.md]
---

Trusted plugins can declare a database namespace via their manifest's `database` block (`namespaceSlug`, `migrationsDir`, `coreReadTables`). The host derives a deterministic schema name from the plugin key, applies the plugin's SQL migrations into that schema, and exposes runtime access through `ctx.db` on the worker side. Plugin SQL may only create or mutate objects inside its own namespace; selected core tables (e.g. `public.issues`) are reachable as read-only join targets enforced at runtime.

Migration state is tracked in two host-owned tables introduced in migration `0059_plugin_database_namespaces`: `plugin_database_namespaces` records the allocated schema, mode, and lifecycle status per installed plugin, and `plugin_migrations` records each applied migration's checksum, plugin version, and status. Both cascade on plugin deletion. Migration replay is idempotent — re-running migration 0059 against a database where the tables already exist must succeed (covered by `client.test.ts`).

Namespace migrations and startup schema checks use `DATABASE_MIGRATION_URL` when set (a direct, unpooled connection), while the application continues to use `DATABASE_URL` for runtime queries. This split exists so hosted deployments behind a connection pooler can still run DDL safely.

Required capabilities for plugins are `database.namespace.migrate`, `database.namespace.read`, and `database.namespace.write`. Without these, the host refuses to allocate a namespace or expose `ctx.db`.
