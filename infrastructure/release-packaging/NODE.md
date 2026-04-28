---
title: "Release Packaging for Standalone Public Packages"
owners: [bingran-you, cryppadotta, serenakeyitan, devinfoley]
soft_links: [engineering/contributor-guide/NODE.md, infrastructure/ci-cd/NODE.md, infrastructure/deployment/NODE.md]
---

Release-time tooling that prepares standalone public packages for publishing. Lives in `scripts/build-standalone-public-packages.mjs` and is invoked from `scripts/release.sh` as Step 2/7 (workspace artifact build), after `pnpm build` and before `prepare-server-ui-dist.sh`.

## Purpose

During release, some workspace packages are published as standalone public packages and must be repackaged so their published form does not depend on the monorepo's pnpm workspace layout. This script discovers those packages by reading `pnpm-workspace.yaml` and the release package map (`scripts/release-package-map.mjs`), then performs the standalone build for each.

## How it fits into the release flow

`scripts/release.sh` runs the build pipeline in numbered steps. This script is wired in at Step 2/7 immediately after `pnpm build`, ensuring standalone public packages are produced from freshly built workspace artifacts before downstream steps (e.g. `prepare-server-ui-dist.sh`, adapter skill bundling) run.

## Workspace discovery

The script parses `pnpm-workspace.yaml` using a line-based matcher tied to the repo's current block-sequence `packages:` format. If that file moves to a more complex YAML shape, the parser must be swapped for a real YAML parser. Negated entries (`!pattern`) are honored, and glob patterns are converted to regular expressions for matching package directories.

## Related

See `infrastructure/deployment/` for deployment flows that consume the published packages, and `infrastructure/ci-cd/` for the surrounding release automation.
