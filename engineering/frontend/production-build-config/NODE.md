---
title: "Production Build Config"
owners: [bingran-you, cryppadotta, serenakeyitan, stubbi]
---

# Production Build Config

Conventions applied to the frontend Vite build that shape what ships to users versus what stays in development. These are global build-time transforms configured in `ui/vite.config.ts`, not per-component opt-ins.

## Key Decisions

### Drop `console.*` and `debugger` in Production Builds

Production builds strip all `console.*` calls and `debugger` statements (via esbuild's `drop: ["console", "debugger"]`). Developers can leave `console.log`/`console.warn`/`console.error` and ad-hoc `debugger` breakpoints in source freely for local debugging without worrying that they ship to end users — the build removes them. This keeps the deployed bundle quieter (no noisy diagnostic output in user devtools, no accidental debugger breaks) and slightly smaller, while preserving the ergonomic value of `console.*` and `debugger` during development.

**Implication for contributors:** do not rely on `console.*` or `debugger` as a runtime side effect in production code paths. If a log line must reach production (telemetry, audit), route it through the appropriate logging utility instead of `console`.

### Strip Legal Comments

Production builds also strip legal/license comments from the bundled output. This trims bundle size at the cost of inlined license headers. Upstream license obligations are satisfied through the repository's own license files and dependency manifests rather than per-bundle inline comments.

## Scope

These transforms apply only to production builds. Development builds (`vite dev`) preserve `console.*`, `debugger`, and comments so the developer experience is unaffected.
