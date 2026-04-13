---
title: "CLI Dev Boot Ordering"
owners: [@bingran-you, @cryppadotta, @serenakeyitan]
---

# CLI Dev Boot Ordering

The CLI development server (`cli dev`) requires specific package preparation ordering before it can boot successfully. This captures the cross-package build dependencies that must be satisfied during local development.

## Key Decisions

### Plugin SDK Preparation Before Dev Boot

The plugin SDK (`@paperclipai/plugin-sdk`) must be built/prepared before the CLI dev server starts. The CLI imports plugin SDK types and utilities at boot time, and without a prior build step, TypeScript compilation fails or runtime imports are missing. This was surfaced as a regression when the dev runner attempted to boot without the SDK artifacts in place.

### Why This Matters

Paperclip is a pnpm monorepo where packages reference each other via workspace protocol. During production builds, the full dependency graph is resolved by the build pipeline. During development with `tsx`, however, some packages need their build artifacts to exist before dependents can import them. The plugin SDK is one such package — its exports include generated types and bundler presets that are not available from raw source alone.

## Boundaries

This node covers dev-time boot ordering only. Production build ordering is handled by the CI/CD pipeline and is not affected by this constraint. The plugin SDK's API surface is documented in `../../plugins/sdk/NODE.md`.
