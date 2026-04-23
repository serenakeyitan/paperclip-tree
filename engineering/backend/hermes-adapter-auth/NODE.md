# Hermes Adapter Auth Injection

The Hermes local adapter (`hermes_local`) requires special handling in the server adapter registry to ensure agent identity attribution flows through to Paperclip API calls the Hermes runtime makes on behalf of the agent.

## Why

`hermes-paperclip-adapter` v0.2.0 predates the shared `authToken` field on `AdapterExecutionContext`. Without injection, Hermes-run agents would make Paperclip API calls without the per-agent JWT, breaking identity attribution on writes (comments, issue updates, etc.).

## How it works

In `server/src/adapters/registry.ts`, the `hermes_local` adapter wraps `hermesExecute` with a shim that, when an `authToken` is present on the execution context:

- Injects `PAPERCLIP_API_KEY` into the adapter's `env` (unless the agent's `adapterConfig.env` already sets it explicitly).
- Injects `PAPERCLIP_RUN_ID` into the adapter's `env` so mutating requests can be correlated to the originating run.
- Prepends an auth-guard prompt to the adapter's `promptTemplate` instructing the agent to always send `Authorization: Bearer $PAPERCLIP_API_KEY` on Paperclip API requests and `X-Paperclip-Run-Id: $PAPERCLIP_RUN_ID` on any write, and to never use a board/browser/local-board session for Paperclip API writes.

If the caller has already set `PAPERCLIP_API_KEY` in `adapterConfig.env`, the explicit value wins and the injected token is skipped.

## Scope

This shim is Hermes-specific. Other adapters consume `authToken` directly via their typed execution context. The cast to `ServerAdapterModule["execute"]` in `registry.ts` is intentional and should be removed once Hermes ships a matching `AdapterExecutionContext` type with the `authToken` field.
