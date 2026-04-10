---
title: "CI/CD"
owners: [cryppadotta, serenakeyitan]
soft_links: ["infrastructure/deployment/NODE.md", "infrastructure/testing/NODE.md"]
---

# CI/CD

GitHub Actions workflows, release pipeline, and automation for the Paperclip monorepo.

---

## Workflows

### PR Pipeline (`pr.yml`)

Runs on every pull request to `master`. Three parallel jobs after a policy gate:

1. **`policy`** -- Fast (5 min timeout). Blocks manual lockfile edits, validates the Dockerfile deps stage matches all workspace packages, and checks dependency resolution when manifests change.
2. **`verify`** -- Typecheck, unit tests, build, and a canary dry-run to catch release script regressions.
3. **`e2e`** -- Full Playwright end-to-end suite in `local_trusted` mode with embedded PGlite. Uploads Playwright reports as artifacts (14-day retention).

Uses concurrency groups keyed on PR number with `cancel-in-progress: true` -- new pushes kill stale runs.

### Release Pipeline (`release.yml`)

Two triggers, two paths:

- **Push to `master`** -- Auto-publishes a **canary** release. Verify -> publish to npm under `canary` dist-tag -> push `canary/v*` git tag.
- **`workflow_dispatch`** -- Manual **stable** release. Accepts a source ref and optional date. Supports dry-run. Publishes under `latest` dist-tag, creates `v*` git tag, and creates a GitHub Release with release notes.

Version scheme: `YYYY.MDD.P` where M is month, DD is zero-padded day, P is same-day patch slot. Canary appends `-canary.N`. The release script (`scripts/release.sh`) orchestrates a 7-step process: verify -> build artifacts -> rewrite versions -> build CLI bundle -> publish to npm -> verify npm availability -> create git tag. Versions are rewritten temporarily and restored via exit trap.

### Docker Build (`docker.yml`)

Triggers on push to `master` and `v*` tags. Builds multi-arch (`amd64`/`arm64`) images with Buildx. Pushes to `ghcr.io` with semantic version tags, SHA tags, and `latest` on the default branch. Uses GHA cache for layer reuse.

### Lockfile Refresh (`refresh-lockfile.yml`)

Triggers on push to `master`. Runs `pnpm install --lockfile-only`, and if the lockfile changed, opens (or updates) a PR from `chore/refresh-lockfile` branch. Auto-enables squash merge. This keeps the lockfile in sync without developers needing to touch it manually.

### E2E (standalone) (`e2e.yml`)

Manual `workflow_dispatch` trigger with an option to skip LLM-dependent assertions. Used for ad-hoc e2e runs outside the PR pipeline. Can use real API keys from secrets when `skip_llm` is false.

### Release Smoke (`release-smoke.yml`)

Post-publish verification. Launches a Docker container from the published npm package (via `Dockerfile.onboard-smoke`), runs the onboard flow, then executes a Playwright smoke suite against the running instance. Validates that the published artifact actually works end-to-end. Can be called from other workflows or dispatched manually.

---

## Key Decisions

### CI owns the lockfile

PRs cannot modify `pnpm-lock.yaml`. The `refresh-lockfile` workflow is the only path for lockfile updates. This prevents merge conflicts and ensures reproducible installs. The policy job enforces this with an explicit branch-name check (`chore/refresh-lockfile` is exempt).

### Canary on every merge, stable on demand

Every merge to `master` produces a canary npm release automatically. Stable releases are manual and require pre-written release notes at `releases/v*.md`. This gives fast feedback on canary while keeping stable releases deliberate.

### Dockerfile deps stage validation

The PR pipeline validates that every workspace `package.json` is listed in the Dockerfile `deps` stage. This catches the common mistake of adding a new package but forgetting to update the Dockerfile, which would cause Docker builds to fail with missing dependencies.

### Concurrency controls

- PR: cancel-in-progress per PR number
- Release: no cancel-in-progress (releases must not be interrupted)
- Docker: cancel-in-progress per ref
- Lockfile refresh: no cancel-in-progress (only one refresh at a time)

## Decision Records

- [ci-owns-the-lockfile.md](ci-owns-the-lockfile.md) — Why lockfile updates are centralized in automation instead of ordinary pull requests.

---

## Toolchain Versions

| Tool | Version | Notes |
|---|---|---|
| Node.js | 24 (CI) / LTS (Docker) | CI uses 24 for latest features; Docker pins LTS for stability |
| pnpm | 9.15.4 | Pinned in CI and `packageManager` field |
| Playwright | ^1.58.2 | Chromium only -- no Firefox/WebKit in CI |
