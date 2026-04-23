---
title: "Dev Server Status Token"
owners: [bingran-you, cryppadotta, serenakeyitan]
---

# Dev Server Status Token

The dev runner and backend share a per-process secret that gates access to supervisor-only dev-server metadata on `/api/health`. The runner mints a random UUID at startup (`scripts/dev-runner.ts` / `.mjs`) and injects it into the child server as `PAPERCLIP_DEV_SERVER_STATUS_TOKEN`; the backend exposes dirty-state, pending migrations, and last-restart-at only when the request carries the matching `x-paperclip-dev-server-status-token` header.

## Key Decisions

### Dev Mode Only

The token is generated only when the runner is in `dev` mode. In `watch` mode the env var is explicitly deleted so the embedded server does not expose supervisor metadata to any caller — dev-only diagnostics must never leak into watch or production runtimes.

### Header, Not Cookie

The runner attaches the token as a request header rather than a cookie so it cannot collide with Better Auth's instance-scoped cookies and is not subject to the 127.0.0.1 host-wide cookie jar used by worktree servers.

## Related

- Produced by the dev runner scripts under `engineering/backend/dev-runner/`.
- Consumed by the health route in `server/src/routes/health.ts` (see `engineering/backend/`).

Source PR: paperclipai/paperclip#4087.
