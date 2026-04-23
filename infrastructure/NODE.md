---
title: "Infrastructure"
owners: [bingran-you, cryppadotta, serenakeyitan]
soft_links: ["engineering/NODE.md", "adapters/NODE.md"]
---

# Infrastructure

Deployment, Docker, CI/CD pipelines, testing strategy, and operational concerns for the Paperclip monorepo.

---

## Sub-domains

- **[deployment/](deployment/NODE.md)** -- Docker images, compose configs, deployment modes, and entrypoint logic.
- **[ci-cd/](ci-cd/NODE.md)** -- GitHub Actions workflows, release pipeline, lockfile management.
- **[testing/](testing/NODE.md)** -- Vitest unit tests, Playwright e2e, promptfoo evals, release smoke tests.
- **[backups/](backups/NODE.md)** -- Automated database snapshots, compression, tiered retention, and backup configuration.
- **[discord-daily-digest/](discord-daily-digest/NODE.md)** -- Daily merge digest posted from `master` to a Discord webhook by `scripts/discord-daily-digest.sh`.

---

## Key Decisions

### Monorepo tooling: pnpm workspaces

The entire codebase is a single pnpm workspace (`pnpm-workspace.yaml`). All CI and Docker builds use `pnpm install --frozen-lockfile`. The lockfile is CI-owned -- PRs that manually edit `pnpm-lock.yaml` are rejected by policy. A dedicated `refresh-lockfile` workflow auto-creates PRs when dependencies drift.

### Two deployment modes

Paperclip runs in two distinct modes, controlled by `PAPERCLIP_DEPLOYMENT_MODE`:

- **`local_trusted`** -- For local dev. No auth required. Embedded PGlite database. Used in e2e CI tests.
- **`authenticated`** -- For Docker/production. Requires `BETTER_AUTH_SECRET`. Uses external Postgres. Default in all Docker images.

This is a load-bearing distinction: the server conditionally enables auth middleware, the UI renders login flows, and e2e tests skip auth-dependent assertions based on this mode.

### Node.js LTS on Debian Trixie

All Docker images use `node:lts-trixie-slim` as the base. The production image installs Claude Code, Codex, and opencode-ai globally -- these are the agent runtimes the server orchestrates.

### UID/GID remapping for volume permissions

The Docker entrypoint (`docker-entrypoint.sh`) dynamically remaps the `node` user's UID/GID to match the host user via `USER_UID`/`USER_GID` build args and environment variables. This avoids permission conflicts on bind-mounted `/paperclip` volumes. Uses `gosu` to drop privileges after remapping.

---

## Gotchas

- The Dockerfile `deps` stage must list every workspace `package.json` individually. CI validates this on every PR (`Validate Dockerfile deps stage` step). Forgetting a new package here breaks Docker builds silently.
- `pnpm install --no-frozen-lockfile` is used in release workflows (not `--frozen-lockfile`) because the release process may run against commits where the lockfile hasn't been refreshed yet.
- The release script temporarily rewrites all workspace package versions and restores the working tree on exit via a trap. Interrupting a release can leave dirty state -- `cleanup_release_state()` handles this.
