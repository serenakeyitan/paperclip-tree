---
title: "Board Mutation Guard"
owners: [bingran-you, cryppadotta, serenakeyitan, stubbi]
---

# Board Mutation Guard

Middleware that protects board-mutating routes from cross-origin requests. Source: `server/src/middleware/board-mutation-guard.ts`.

## Trusted Origins

The guard validates the request `Origin` header against a set of trusted origins. In addition to same-origin requests, the value of the `PAPERCLIP_PUBLIC_URL` environment variable is treated as a trusted origin. This allows deployments behind a reverse proxy or with a public-facing hostname distinct from the internal bind address to accept board mutations from their own UI without spurious rejections.

## Why PAPERCLIP_PUBLIC_URL

When Paperclip is deployed behind a proxy or with a custom domain, the browser's `Origin` header reflects the public URL, while the server's own host may be an internal address. Without trusting `PAPERCLIP_PUBLIC_URL`, legitimate board mutations from the configured public UI would be blocked by the guard. Operators configure this variable as part of standard deployment; the guard reads it at request time.

## Scope

This guard applies only to board mutation routes. Read-only endpoints and non-board routes are unaffected. Any change to the trusted-origin policy should be made here so the rule stays in one place.
