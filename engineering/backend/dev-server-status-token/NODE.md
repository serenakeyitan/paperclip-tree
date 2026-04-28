---
title: "Dev Server Status Token"
owners: [bingran-you, cryppadotta, serenakeyitan]
soft_links: [engineering/backend/NODE.md, engineering/backend/dev-runner/NODE.md]
---

# Dev Server Status Token

The dev runner (`scripts/dev-runner.mjs` / `scripts/dev-runner.ts`) generates a per-process `randomUUID` at startup in `dev` mode and exports it as `PAPERCLIP_DEV_SERVER_STATUS_TOKEN`. The server's health route (`server/src/routes/health.ts`) only includes dev-server status metadata (dirty flag, changed paths, pending migrations, last restart) when the request carries that token in the `x-paperclip-dev-server-status-token` header.

## Key Decisions

### Token-Gated Dev Metadata

Dev-server status is operationally sensitive (file paths, migration state) and must not leak to unauthenticated clients hitting `/api/health`. Gating it behind a per-launch token means only the supervising dev runner can read it, while the public health probe stays usable for liveness checks.

### Watch Mode Strips the Token

`watch` mode explicitly `delete`s `PAPERCLIP_DEV_SERVER_STATUS_TOKEN` from the spawned env so background watch processes can't impersonate the dev runner.

### Tested Contract

Covered by `server/src/__tests__/health-dev-server-token.test.ts` — both the authenticated path (token present, metadata returned) and the unauthenticated path are exercised.
