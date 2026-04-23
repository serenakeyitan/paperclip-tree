---
title: "Test Command Tiers"
owners: [bingran-you, cryppadotta, serenakeyitan]
soft_links: ["infrastructure/testing/NODE.md", "infrastructure/testing/no-llm-calls-in-default-ci.md"]
---

# Test Command Tiers

Paperclip's test commands are split into tiers so that the default local and agent test path stays cheap. Contributors and agents should run `pnpm test` by default; browser suites are opt-in and run only when the change touches them or when explicitly verifying CI/release flows.

## Key Decisions

### `pnpm test` Runs Vitest Only

The default `pnpm test` command runs the Vitest suite and nothing else. It does not run Playwright. This keeps the inner loop fast for both humans and agents and prevents unrelated browser flakiness from blocking unrelated work. `pnpm test:run` is the explicit non-watch variant (and `pnpm test` aliases it); `pnpm test:watch` is the interactive Vitest mode.

### Browser Suites Stay Separate and Opt-In

Playwright and release-smoke browser tests are exposed as distinct commands (`pnpm test:e2e`, `pnpm test:release-smoke`) rather than being folded into `pnpm test`. Run them when your change touches browser flows or when you are explicitly verifying CI/release behavior. CI still runs them in the appropriate pipelines, but local and agent defaults do not.

### Verification Before Hand-off

The `AGENTS.md` hand-off checklist uses the cheap default (`pnpm test`). Agents should not escalate to browser suites unless the diff touches UI end-to-end flows, because those suites are slower and noisier and will produce false negatives on unrelated changes.

## Source

- `package.json` — script definitions
- `AGENTS.md`, `README.md`, `cli/README.md`, `doc/DEVELOPING.md` — contributor guidance
- paperclipai/paperclip#3679 — PR that established this convention
