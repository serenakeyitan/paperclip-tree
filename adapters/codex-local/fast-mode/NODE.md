---
title: "Codex Fast Mode"
owners: [@bingran-you, @cryppadotta, @serenakeyitan]
---

# Codex Fast Mode

The Codex local adapter supports a **fast mode** flag that enables faster execution at potentially reduced quality. Added in PR #3383.

## Key Decisions

### Fast Mode Is Opt-In Per Execution

Fast mode is passed as a flag to the Codex CLI. It is not a global adapter setting — it can be toggled per heartbeat run based on task priority or context.

### Environment Probe Exclusion

Fast mode is explicitly disabled during environment probes (`testEnvironment`). Probes need reliable, deterministic output to validate CLI installation and auth state. Fast mode's speed-vs-quality tradeoff is inappropriate for diagnostics.

## Boundaries

- Fast mode behavior is defined by the Codex CLI, not by Paperclip. The adapter only passes the flag through.
- No Paperclip-side quality assessment of fast-mode outputs. The agent is responsible for its own output quality.
