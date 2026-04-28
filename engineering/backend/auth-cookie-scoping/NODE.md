---
title: "Better Auth Cookie Scoping by Instance"
owners: [bingran-you, cryppadotta, serenakeyitan]
soft_links: [engineering/backend/NODE.md, engineering/backend/dev-runner/worktree-dev-tooling/NODE.md, engineering/execution-workspaces/NODE.md]
---

# Better Auth Cookie Scoping by Instance

Authenticated Paperclip servers derive their Better Auth `cookiePrefix` from `PAPERCLIP_INSTANCE_ID` via `deriveAuthCookiePrefix` in `server/src/auth/better-auth.ts` (e.g. instance `sat-worktree` → cookie prefix `paperclip-sat-worktree`, session cookie `paperclip-sat-worktree.session_token`).

## Why

Browser cookies are scoped by host, not port. Multiple worktree dev servers all run on `127.0.0.1:<port>`, so without an instance-scoped prefix, logging into one worktree would overwrite the session cookie of another. Prefixing every auth cookie with the instance ID keeps each worktree's session isolated in the same browser.

## Key Decisions

### `disableSecureCookies` Is Independent of Prefix

`buildBetterAuthAdvancedOptions({ disableSecureCookies })` toggles the Secure flag for local HTTP development but always preserves the instance-scoped prefix. Local dev gets non-secure cookies without losing isolation between worktrees.

### Tested Contract

Covered by `server/src/__tests__/better-auth.test.ts`, which pins both the prefix derivation and the resulting cookie names.
