---
title: "Heartbeat Verification Scope"
owners: [bingran-you, cryppadotta, serenakeyitan]
soft_links: [engineering/contributor-guide/NODE.md, infrastructure/testing/test-command-tiers/NODE.md, product/agent-model/instructions-bundle/NODE.md]
---

# Heartbeat Verification Scope

Agents working a Paperclip issue should run the **smallest verification that proves the change**, not the full repo-wide `pnpm -r typecheck` / build / test suite on every heartbeat. The full check is reserved for PR-ready hand-offs or for changes whose scope is broad enough that targeted checks cannot prove correctness.

## Key Decisions

### Smallest Sufficient Check by Default

For a typical issue heartbeat — a localized bug fix, a single-package change, a UI tweak — the agent should run only the package-level or file-level verification that exercises the change (e.g. one vitest file, one package's typecheck). Repo-wide checks are expensive and create noisy retry/backoff behavior when they fail for unrelated reasons.

### Full Suite Before PR Hand-Off

Before claiming repo work "done" in a PR-ready hand-off, or when the change touches shared types / adapter contracts / cross-package interfaces, the agent must still run the full `pnpm -r typecheck`, build, and test sequence documented in AGENTS.md. Broad changes are exactly the case where targeted checks are not sufficient.

### Encoded in the Default Wake Prompt

The default Paperclip agent prompt template (`DEFAULT_PAPERCLIP_AGENT_PROMPT_TEMPLATE` in `packages/adapter-utils/src/server-utils.ts`) carries this rule as part of the execution contract, so every adapter that uses the default prompt inherits it without per-adapter changes.

## Why This Exists

Defaulting to full-repo verification on every heartbeat wastes wall-clock time, masks the real signal of a change, and inflates upstream usage against rate-limited providers. Sizing verification to the change keeps heartbeats fast and keeps test failures meaningful.
