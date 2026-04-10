---
title: "Authenticated Docker And Local Trusted Dev"
owners: [bingran-you, cryppadotta, serenakeyitan]
---

# Authenticated Docker And Local Trusted Dev

Paperclip needs safe defaults in networked deployments without making local
development and onboarding unnecessarily heavy.

## Decision

Use two load-bearing deployment modes: Docker and hosted-style environments
default to `authenticated` mode with external Postgres, while local development
and local e2e workflows use `local_trusted` mode with embedded database flows.

## Why

This preserves a low-friction developer loop while making authentication and
clear environment boundaries the default for any deployment that is exposed
beyond a single trusted local machine.

## Implications

- Auth, API access, and UI flows must always respect deployment mode instead of
  assuming one universal runtime posture.
- Compose and production entrypoints should fail fast when required auth
  configuration is missing.
- Local tests can stay simple and deterministic by relying on embedded storage
  and trusted-mode shortcuts.
- Feature work that behaves differently across trusted and authenticated modes
  should make that distinction explicit in both code and docs.

## Related Domains

- [engineering backend](../../engineering/backend/NODE.md)
- [product governance](../../product/governance/NODE.md)
- [infrastructure testing](../testing/NODE.md)
