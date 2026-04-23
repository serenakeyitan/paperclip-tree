---
title: "Hermes Adapter Auth Injection"
owners: [bingran-you, cryppadotta, serenakeyitan]
soft_links: ["adapters/shared-server-adapter-contract.md", "engineering/backend/shared-api-and-actor-scoped-authorization.md"]
---

# Hermes Adapter Auth Injection

The Hermes local adapter (`hermes_local`) needs special handling in the server
adapter registry so agent identity attribution flows through to Paperclip API
calls the Hermes runtime makes on behalf of the agent.

## Why

`hermes-paperclip-adapter` predates the shared `authToken` field on
`AdapterExecutionContext`. Without injection, Hermes-run agents would make
Paperclip API calls under a server or board-user identity (or with no run id),
breaking per-agent attribution on mutating writes (comments, issue updates,
etc.) and making audit trails misleading.

## How it works

In `server/src/adapters/registry.ts`, the `hermes_local` adapter wraps the
underlying `hermesExecute` with a shim that runs when an `authToken` is present
on the execution context:

- Injects `PAPERCLIP_API_KEY` into the adapter's `env` with the run-scoped
  agent JWT, unless the agent's `adapterConfig.env` already sets it explicitly.
- Injects `PAPERCLIP_RUN_ID` into the adapter's `env` from `ctx.runId` so
  mutating requests can be correlated to the originating run.
- Prepends an auth-guard instruction to any *custom* `promptTemplate` telling
  the agent to send `Authorization: Bearer $PAPERCLIP_API_KEY` on Paperclip API
  requests and `X-Paperclip-Run-Id: $PAPERCLIP_RUN_ID` on writes, and to never
  reuse a board/browser/local-board session for Paperclip API writes.

Explicit caller configuration wins: if `adapterConfig.env.PAPERCLIP_API_KEY` is
already set, the injected token is skipped. When no custom `promptTemplate`
exists, Hermes' built-in default prompt is left intact — the auth guard is only
prepended to custom templates so unconfigured agents keep Hermes' default
heartbeat/task instructions.

## Scope

This shim is Hermes-specific. Other adapters consume `authToken` directly via
their typed execution context. The `as unknown as ServerAdapterModule["execute"]`
cast in `registry.ts` is intentional and should be removed once
`hermes-paperclip-adapter` ships an `AdapterExecutionContext` type that
includes `authToken`.

## Source

Introduced in paperclipai/paperclip#3608 — *fix(hermes): inject agent JWT into
Hermes adapter env to fix identity attribution*.
