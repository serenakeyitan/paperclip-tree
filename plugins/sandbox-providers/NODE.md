---
title: "Sandbox Provider Plugins"
owners: [bingran-you, cryppadotta, serenakeyitan, devinfoley]
soft_links: [engineering/execution-workspaces/NODE.md, infrastructure/ci-cd/NODE.md, infrastructure/deployment/NODE.md, plugins/NODE.md, plugins/examples/NODE.md, plugins/runtime/NODE.md, plugins/sdk/NODE.md]
---

Sandbox-provider plugins live under `packages/plugins/sandbox-providers/` and provision execution environments (cloud sandboxes, container hosts, etc.) for Paperclip via the plugin SDK's `environment.drivers.register` capability. They register one or more `environmentDrivers` of kind `sandbox_provider` in their manifest and implement environment lifecycle hooks (probe, validateConfig, acquireLease, resumeLease, releaseLease, destroyLease, realizeWorkspace, execute) using `definePlugin` from `@paperclipai/plugin-sdk`.

Unlike example plugins, sandbox providers are intentionally **excluded from the root pnpm workspace** (`!packages/plugins/sandbox-providers/**` in `pnpm-workspace.yaml`). They are shaped to publish and install like standalone npm packages so that operators can install them by package name from the Plugins page and the host's plugin installer can `npm install` their transitive third-party deps at install time, without adding lockfile churn to the Paperclip repo. Local development uses `pnpm install --ignore-workspace --no-lockfile` after a one-time root install so the local `@paperclipai/plugin-sdk` workspace package is available to the compiler.

Publishing goes through `scripts/generate-plugin-package-json.mjs`, which produces a slimmed `package.json` for npm publish: it pins `@paperclipai/plugin-sdk` to the SDK workspace's current version and lifts `publishConfig.exports/main/types` to the top level so the published package points at `dist/` while local development still resolves through `src/`. The host reads `paperclipPlugin.manifest` and `paperclipPlugin.worker` to locate built entrypoints.

Docker build support is provided by `COPY --parents packages/plugins/sandbox-providers/./*/package.json` in the deps stage (requires `# syntax=docker/dockerfile:1.20`), and `scripts/check-docker-deps-stage.mjs` replaces the inline shell validator in `.github/workflows/pr.yml` to verify the deps stage stays in sync with the workspace, including the sandbox-providers carve-out.

The first member of this category is `@paperclipai/plugin-e2b` (E2B cloud sandboxes), which registers a `sandbox_provider` driver with configurable template, secret-ref API key, timeout, and lease reuse, and maps E2B's `CommandExitError`, `SandboxNotFoundError`, and `TimeoutError` onto the SDK's environment lifecycle contract.
