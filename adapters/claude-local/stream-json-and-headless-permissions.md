---
title: "Stream JSON And Headless Permissions"
owners: [cryppadotta, serenakeyitan]
---

# Stream JSON And Headless Permissions

The Claude adapter needs to run unattended inside Paperclip while still
surfacing enough structured information for the control plane to reason about
cost, continuation, and failures.

## Decision

Run Claude Code in structured `stream-json` mode and treat headless permission
behavior as an explicit adapter concern instead of relying on plain-text output
or hidden runtime defaults.

## Why

Structured output provides stable access to session IDs, usage, cost, and turn
results. Explicitly modeling `--dangerously-skip-permissions` and related login
flows makes the difference between local interactive usage and server-driven
automation visible instead of implicit.

## Implications

- The adapter can parse usage and continuation signals without brittle regex
  scraping.
- Operational setup for Claude agents includes both auth state and headless
  permission posture, not just binary availability.
- The server can distinguish max-turn or continuation-style outcomes from hard
  execution failures and react differently.
- Claude-specific login/bootstrap flows remain in the adapter boundary rather
  than leaking into general orchestration logic.

## Related Domains

- [adapters](../NODE.md)
- [engineering backend](../../engineering/backend/NODE.md)
- [product agent model](../../product/agent-model/NODE.md)
