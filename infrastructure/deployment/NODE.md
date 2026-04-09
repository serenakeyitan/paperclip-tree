---
title: "Deployment"
owners: [cryppadotta, serenakeyitan]
soft_links: ["engineering/NODE.md", "infrastructure/ci-cd/NODE.md"]
---

# Deployment

Docker images, compose configurations, deployment modes, and container orchestration for Paperclip.

---

## Docker Architecture

### Multi-stage Dockerfile (`/Dockerfile`)

Four stages, each building on a common `base`:

1. **`base`** -- `node:lts-trixie-slim` with system deps (git, gh, ripgrep, python3, curl). Enables corepack for pnpm. Remaps `node` user UID/GID to match host.
2. **`deps`** -- Copies every workspace `package.json` individually (not the full source), then runs `pnpm install --frozen-lockfile`. This maximizes Docker layer caching -- source changes don't bust the dependency cache.
3. **`build`** -- Copies full source, builds UI, plugin SDK, and server. Includes a build verification gate (`test -f server/dist/index.js`).
4. **`production`** -- Copies the built app. Installs agent runtimes globally (`claude-code`, `codex`, `opencode-ai`). Sets production defaults. Runs via `docker-entrypoint.sh` with `gosu` privilege drop.

### Compose Configurations (`/docker/`)

Three compose files for different use cases:

- **`docker-compose.yml`** -- Full stack: Postgres 17 + Paperclip server. Authenticated mode. Requires `BETTER_AUTH_SECRET` env var (fails fast if missing). Health-checked Postgres dependency.
- **`docker-compose.quickstart.yml`** -- Single-container quickstart. No external Postgres (uses embedded). Bind-mounts a local data directory. Authenticated mode with configurable port.
- **`docker-compose.untrusted-review.yml`** -- Sandboxed code review container. Drops ALL capabilities, sets `no-new-privileges`. Runs as non-root `reviewer` user. Separate Dockerfile at `docker/untrusted-review/Dockerfile` installs Claude Code and Codex for PR review workflows.

### Quadlet Files (`/docker/quadlet/`)

Systemd quadlet definitions for running Paperclip as a Podman pod on Linux hosts. Defines a `paperclip` pod publishing port 3100.

### Onboard Smoke Image (`/docker/Dockerfile.onboard-smoke`)

Separate Ubuntu-based image that tests the `npx paperclipai@<version> onboard` flow end-to-end. Used by the release-smoke CI workflow. Not the same base as the main Dockerfile -- intentionally uses a clean Ubuntu environment to simulate a real user's first install.

---

## Key Decisions

### Postgres 17 for production, PGlite for dev

Production Docker runs external Postgres 17 (Alpine). Local dev uses embedded PGlite (in-process, zero config). The `database.mode` config field controls this: `"embedded-postgres"` for local, external `DATABASE_URL` for Docker.

### Authenticated vs. local_trusted

Docker defaults to `PAPERCLIP_DEPLOYMENT_MODE=authenticated` with `PAPERCLIP_DEPLOYMENT_EXPOSURE=private`. This enables Better Auth with JWT-based sessions. The `BETTER_AUTH_SECRET` is required (compose uses `${BETTER_AUTH_SECRET:?...}` to fail fast). Local dev defaults to `local_trusted` which bypasses auth entirely.

### Security-hardened review container

The untrusted-review container is designed for running AI agents on untrusted code. It drops ALL Linux capabilities, prevents privilege escalation, uses a tmpfs for `/tmp`, and runs as a dedicated `reviewer` user. This is a deliberate security boundary -- the review container should never be relaxed without security review.

### Multi-arch Docker builds

CI builds `linux/amd64` and `linux/arm64` images. Uses Docker Buildx with GitHub Actions cache (`type=gha`). Published to GitHub Container Registry (`ghcr.io`).

---

## Environment Variables (Production Defaults)

| Variable | Default | Purpose |
|---|---|---|
| `PAPERCLIP_DEPLOYMENT_MODE` | `authenticated` | Auth mode |
| `PAPERCLIP_DEPLOYMENT_EXPOSURE` | `private` | Network exposure |
| `PORT` | `3100` | Server listen port |
| `SERVE_UI` | `true` | Serve built UI from server |
| `PAPERCLIP_HOME` | `/paperclip` | Data volume root |
| `USER_UID` / `USER_GID` | `1000` | Container user mapping |
