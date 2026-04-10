---
title: "Testing"
owners: [bingran-you, cryppadotta, serenakeyitan]
soft_links: ["infrastructure/ci-cd/NODE.md", "engineering/NODE.md"]
---

# Testing

Testing strategy, frameworks, and test organization for Paperclip.

---

## Test Layers

### Unit Tests (Vitest)

- Framework: Vitest (`^3.0.5`)
- Run: `pnpm test:run` (single run), `pnpm test` (watch mode)
- Scope: Package-level logic -- shared utilities, server business logic, adapter behavior
- Location: Co-located with source or in package-level test directories
- Runs in: PR pipeline `verify` job and release `verify_canary`/`verify_stable` jobs

### End-to-End Tests (Playwright)

- Framework: Playwright (`^1.58.2`), Chromium only
- Config: `tests/e2e/playwright.config.ts`
- Run: `pnpm test:e2e`
- Location: `tests/e2e/`
- Test files: `onboarding.spec.ts`, `signoff-policy.spec.ts`

Key design choice: e2e tests run in `local_trusted` deployment mode with embedded PGlite. No external database, no auth. The `PAPERCLIP_E2E_SKIP_LLM=true` flag skips assertions that require real LLM API calls, making CI deterministic without API keys.

The signoff-policy spec explicitly checks deployment mode and fails with a clear message if accidentally run against an authenticated server -- this is a guardrail, not a bug.

### Release Smoke Tests (Playwright)

- Config: `tests/release-smoke/playwright.config.ts`
- Run: `pnpm test:release-smoke`
- Purpose: Post-publish verification of the actual npm package in a Docker container

These test a different thing than e2e: they verify the published artifact works, not the source. The smoke harness (`scripts/docker-onboard-smoke.sh`) launches a Docker container from `Dockerfile.onboard-smoke`, runs the onboard CLI flow, bootstraps an admin user, then the Playwright suite verifies the running instance.

### Agent Evals (promptfoo)

- Framework: promptfoo (`0.103.3`)
- Config: `evals/promptfoo/promptfooconfig.yaml`
- Run: `pnpm evals:smoke`
- Location: `evals/promptfoo/`

Phase 0 "heartbeat" evals testing narrow agent behaviors across multiple models (Claude Sonnet 4, GPT-4.1, Codex 5.4, Gemini 2.5 Pro) via OpenRouter. Tests are organized by category in `tests/*.yaml`. Uses deterministic assertions against structured prompt outputs. Requires `OPENROUTER_API_KEY` -- not run in standard CI.

---

## Key Decisions

### No LLM calls in default CI

Standard CI (PR pipeline, release verify) never calls real LLM APIs. E2e tests skip LLM assertions with `PAPERCLIP_E2E_SKIP_LLM=true`. Agent evals are a separate manual workflow. This keeps CI fast, deterministic, and free of API cost.

### Chromium-only e2e

Playwright is configured for Chromium only -- no Firefox or WebKit. This is a deliberate scope choice to keep CI fast. Cross-browser testing is not a current priority.

### Playwright reports as artifacts

Both e2e and release-smoke workflows upload Playwright HTML reports and test results as GitHub Actions artifacts with 14-day retention. This enables post-hoc debugging of flaky or failed tests without reproducing locally.

### Forbidden token checks

`scripts/check-forbidden-tokens.mjs` scans the codebase for tokens that should never be committed (API keys, secrets). Run via `pnpm check:tokens`. This is a guardrail against accidental secret commits.

## Decision Records

- [no-llm-calls-in-default-ci.md](no-llm-calls-in-default-ci.md) — Why standard CI avoids required live model-provider calls.

---

## Test Commands Reference

| Command | What it runs |
|---|---|
| `pnpm test` | Vitest watch mode |
| `pnpm test:run` | Vitest single run |
| `pnpm test:e2e` | Playwright e2e (headless) |
| `pnpm test:e2e:headed` | Playwright e2e (visible browser) |
| `pnpm test:release-smoke` | Release smoke Playwright suite |
| `pnpm evals:smoke` | promptfoo agent evals |
| `pnpm check:tokens` | Forbidden token scan |
