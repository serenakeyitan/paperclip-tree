---
title: "Codex Fast Mode"
owners: [@bingran-you, @cryppadotta, @serenakeyitan]
---

# Codex Fast Mode

The Codex local adapter supports a **fast mode** flag that enables faster, lower-cost completions when full reasoning is not required.

## Key Decisions

### Fast Mode Pass-Through

When the Paperclip harness or user requests fast mode, the adapter passes the flag through to the Codex CLI. This is a transparent pass-through — Paperclip does not alter the behavior, it simply forwards the preference to the underlying runtime.

### Exclude Fast Mode During Environment Probe

The adapter's environment test (`envTest`) explicitly avoids using fast mode. The env probe validates CLI installation and auth state, and fast mode could alter the probe's behavior or produce misleading results. This ensures the health check always runs against the full-capability path.

Source: `packages/adapters/codex-local/`
