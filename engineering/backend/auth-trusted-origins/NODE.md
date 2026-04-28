---
title: "Auth Trusted Origins & Listen Port Reconciliation"
owners: [bingran-you, cryppadotta, serenakeyitan, devinfoley]
soft_links: [engineering/backend/NODE.md, infrastructure/deployment/NODE.md]
---

`deriveAuthTrustedOrigins` builds the Better Auth `trustedOrigins` allowlist from `config.allowedHostnames` in authenticated deployment mode. For each allowed hostname it always adds `https://<host>` and `http://<host>`, and — when the effective port is not 80 or 443 — also adds `https://<host>:<port>` and `http://<host>:<port>`. This lets browsers reach the server on a non-default port (common in dev and self-hosted setups) without tripping Better Auth's origin check.

The effective port comes from a `listenPort` option passed by the server bootstrap, falling back to `config.port` when not provided. The bootstrap in `server/src/index.ts` resolves the actual listen port via `detectPort(config.port)` *before* constructing Better Auth, then passes that resolved port into `deriveAuthTrustedOrigins`. This matters because `detectPort` may bump the port if the requested one is taken, and the trusted-origin set must match the port the server actually binds to — not the port originally requested in config.

Hostnames are normalized by trimming and lowercasing before being added to the origin set, so case differences in `allowedHostnames` (e.g. `Board.Example.Test`) don't produce duplicate or mismatched entries. Empty hostnames after trimming are skipped.

`createBetterAuthInstance` now requires the caller to pass `trustedOrigins` explicitly rather than deriving them internally, making the listen-port reconciliation the bootstrap's responsibility and keeping the auth module pure with respect to runtime port detection.
