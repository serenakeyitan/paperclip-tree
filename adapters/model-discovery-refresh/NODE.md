---
title: "Adapter Model Discovery Refresh"
owners: [bingran-you, cryppadotta, serenakeyitan]
soft_links: [adapters/NODE.md, adapters/capability-flags/NODE.md, adapters/codex-local/NODE.md, engineering/frontend/NODE.md]
---

# Adapter Model Discovery Refresh

Adapters that discover available models from an upstream provider (e.g. Codex pulling the OpenAI model list) cache results to avoid hammering the provider on every config render. To let users pick up newly released models without waiting for cache expiry or a Paperclip code update, adapters expose an optional `refreshModels()` hook in addition to `listModels()`.

## Key Decisions

### Optional `refreshModels` on `ServerAdapterModule`

`ServerAdapterModule` declares an optional `refreshModels?: () => Promise<AdapterModel[]>`. When present, the server exposes a refresh endpoint that bypasses the adapter's internal cache and re-queries the upstream. Adapters without this hook simply do not support refresh; the UI hides the action.

### UI-Initiated, Server-Mediated

Refresh is triggered from the agent configuration form, routed through an authenticated server route, and dispatched via `refreshAdapterModels(adapterType)` in `server/src/adapters/index.ts`. The server enforces access checks; adapters never expose refresh directly to the client.

### Cache Discipline

Adapters that implement refresh must invalidate their internal cache before re-fetching, so the second call observes the new upstream state. Tests assert this by mocking two distinct upstream responses and verifying that the refreshed result reflects the second one.

## Why This Matters

Providers ship new models frequently (e.g. GPT-5.x revisions). Without an explicit refresh path, users would have to restart Paperclip or wait out cache TTLs to configure agents on new models, which directly blocks adoption of better/cheaper models.
