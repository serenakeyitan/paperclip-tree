---
title: "CLI Mirrors The API And Owns Local Instance Operations"
owners: [bingran-you, cryppadotta, serenakeyitan]
---

# CLI Mirrors The API And Owns Local Instance Operations

Paperclip's CLI is not only a thin wrapper around local scripts, and it is not
only a remote API client. It intentionally does both jobs.

## Decision

Use the CLI as the operator-facing command surface for two categories of work:
local instance lifecycle operations and resource-oriented API client commands
that mirror the server's major HTTP surfaces.

## Why

This lets one tool cover first-run onboarding, diagnostics, local execution,
and headless automation without forcing operators into the browser for routine
control-plane tasks. It also keeps Paperclip scriptable in CI, shell
automation, and dev workflows.

## Implications

- New major server resources should usually get corresponding client
  subcommands instead of remaining UI-only.
- Local-only concerns such as `onboard`, `doctor`, `run`, and `worktree`
  belong in the CLI because they interact with the operator's machine rather
  than only the remote control plane.
- Shared request and config schemas should live in `@paperclipai/shared` so the
  CLI does not drift from server expectations.
- The CLI is part of the product surface, not a secondary developer utility.

## Related Domains

- [engineering backend](../backend/NODE.md)
- [engineering shared](../shared/NODE.md)
- [infrastructure deployment](../../infrastructure/deployment/NODE.md)
