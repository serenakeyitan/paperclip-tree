---
title: "E2B Sandbox Provider Plugin"
owners: [bingran-you, cryppadotta, serenakeyitan, devinfoley]
soft_links: [infrastructure/ci-cd/NODE.md, infrastructure/deployment/NODE.md, plugins/runtime/NODE.md, plugins/sandbox-providers/NODE.md, plugins/sdk/NODE.md]
---

The `@paperclipai/plugin-e2b` package (`packages/plugins/sandbox-providers/e2b/`) — a first-party sandbox provider plugin that provisions E2B cloud sandboxes as Paperclip execution environments. Registers an `e2b` driver of kind `sandbox_provider` via the plugin SDK's `environment.drivers.register` capability and implements the full environment lifecycle (probe, validateConfig, acquireLease, resumeLease, releaseLease, destroyLease, realizeWorkspace, execute) against the `e2b` npm SDK.

## Standalone publish model

Unlike other workspace plugins, sandbox-provider plugins are intentionally **excluded from the root pnpm workspace** (`!packages/plugins/sandbox-providers/**` in `pnpm-workspace.yaml`). This keeps third-party deps like `e2b` out of the root `pnpm-lock.yaml` and lets operators install the plugin from the Plugins page by package name — the host's plugin installer runs `npm install` into the managed plugin directory at install time, fetching transitive deps then.

Local development uses `pnpm install --ignore-workspace --no-lockfile` inside the plugin directory. A shared `scripts/generate-plugin-package-json.mjs` rewrites `package.json` for publish (resolving the workspace `@paperclipai/plugin-sdk` reference to the SDK's current version), and `scripts/link-plugin-dev-sdk.mjs` wires the local SDK during dev builds.

## Driver configuration

The `e2b` driver accepts `template` (E2B sandbox template, default `base`), `apiKey` (Paperclip secret reference, falls back to `E2B_API_KEY` env), `timeoutMs` (default 300_000), and `reuseLease`. Errors from the `e2b` SDK (`CommandExitError`, `SandboxNotFoundError`, `TimeoutError`) are mapped to plugin lifecycle outcomes.

## Docker deps stage

Because sandbox-provider plugin `package.json` files are not part of the workspace but must still be copied into the Docker `deps` stage, the Dockerfile uses BuildKit's `COPY --parents` (requires `# syntax=docker/dockerfile:1.20`) to glob them in. The previous inline shell validator in `.github/workflows/pr.yml` was replaced by `scripts/check-docker-deps-stage.mjs`, which understands the workspace exclusion syntax.
