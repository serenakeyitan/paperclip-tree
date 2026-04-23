---
title: "Dev Server Status Token"
owners: [bingran-you, cryppadotta, serenakeyitan]
---

# Dev Server Status Token

The dev runner and backend share a per-process secret that lets the supervising dev runner read dev-server metadata on `/api/health` without needing a user session. The runner mints a random UUID at startup (`scripts/dev-runner.ts` / `.mjs`) and injects it into the child server as `PAPERCLIP_DEV_SERVER_STATUS_TOKEN`; the backend's health route returns the `devServer` block (dirty-state, pending migrations, last-restart-at) when the caller is already entitled to full details (e.g. board/agent actors) **or** when the request carries the matching `x-paperclip-dev-server-status-token` header. The token is an additional, actor-less path for the dev runner — not the exclusive gate.

## Key Decisions

### Dev Mode Only

The token is generated only when the runner is in `dev` mode. In `watch` mode the env var is explicitly deleted so the embedded server does not expose supervisor metadata to any caller — dev-only diagnostics must never leak into watch or production runtimes.

### Header, Not Cookie

The runner attaches the token as a request header rather than a cookie so it cannot collide with Better Auth's instance-scoped cookies and is not subject to the 127.0.0.1 host-wide cookie jar used by worktree servers.

## Related

- Produced by the dev runner scripts under `engineering/backend/dev-runner/`.
- Consumed by the health route in `server/src/routes/health.ts` (see `engineering/backend/`).

Source PR: paperclipai/paperclip#4087.
