---
title: "Plugin Scoped API Routes"
owners: [bingran-you, cryppadotta, serenakeyitan]
soft_links: [engineering/backend/NODE.md, plugins/examples/NODE.md, plugins/runtime/NODE.md, plugins/sdk/NODE.md]
---

Plugins can expose their own JSON HTTP endpoints by declaring an `apiRoutes` array in the manifest. The host mounts these only under `/api/plugins/:pluginId/api/*`, so plugins cannot shadow core API routes. Routes are JSON-only — no streaming, no HTML, no static asset hosting through this surface.

Declaration requires the `api.routes.register` capability. The host validates declared routes against the manifest at install time and authorizes incoming requests against the installed plugin's capability set (see `plugin-routes-authz.test.ts` and `plugin-scoped-api-routes.test.ts`). Requests that do not match a declared route 404; requests that match but fail capability checks are rejected before reaching plugin code.

Plugin route handlers run inside the worker process and receive the same `ctx` surface as other worker entrypoints, including `ctx.db` when a database namespace is allocated. This is the canonical way for plugins to expose orchestration-style endpoints to their own UI or to external automation without coupling to host route internals.
