---
title: "Vitest Stable Runner"
owners: [bingran-you, cryppadotta, serenakeyitan]
soft_links: [engineering/backend/NODE.md, infrastructure/testing/NODE.md, infrastructure/testing/test-command-tiers/NODE.md]
---

Paperclip's default `pnpm test` / `pnpm test:run` no longer invokes `vitest run` directly. It runs `node scripts/run-vitest-stable.mjs`, a wrapper that orchestrates Vitest across the monorepo to keep the default test path stable under load.

## Why a wrapper

Server route, authz, and several heartbeat/invite/issue test files share global mock state and HTTP listener resources. Running them in Vitest's default parallel mode produced flakes (cross-test mock bleed, port contention, resource exhaustion). The wrapper keeps the inner loop fast for the common case while serializing only the suites that need it.

## How it splits work

- Non-server workspace projects (`@paperclipai/shared`, `@paperclipai/db`, `@paperclipai/adapter-utils`, `@paperclipai/adapter-codex-local`, `@paperclipai/adapter-opencode-local`, `@paperclipai/ui`, `paperclipai`) run in their normal Vitest configuration.
- Server tests under `server/src/__tests__/` matching the route/authz pattern (`*route*.test.ts`, `*routes*.test.ts`, `*authz*.test.ts`) plus an explicit `additionalSerializedServerTests` allow-list (heartbeat, invite, issues, costs, health, portability, etc.) are serialized.
- The remaining server tests run in the normal parallel mode.

## Adding new serialized tests

If a new server test depends on shared mock state, global registries, or its own HTTP listener and proves flaky in parallel mode, add its path to `additionalSerializedServerTests` in `scripts/run-vitest-stable.mjs`. Prefer fixing the test isolation first — many of the route tests in this PR were updated to use `vi.resetModules()` + per-test ephemeral HTTP servers via a local `requestApp()` helper instead of a module-level `let server` — and only fall back to serialization when isolation isn't practical.

## Related

See [Test Command Tiers](infrastructure/testing/test-command-tiers/NODE.md) for the broader split between Vitest (default) and opt-in browser/Playwright suites — this runner sits inside the Vitest tier.
