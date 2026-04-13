---
title: "OpenClaw Gateway Adapter"
owners: []
---

## Per-Agent Configuration Overrides

Gateway-level configuration (auth tokens, API key paths, session strategies) can be overridden on a per-agent basis. This allows a single gateway to serve multiple agents with different credentials and routing.

- **`claimedApiKeyPath`** is configurable per agent, not just at the adapter level. This supports multi-tenant gateway setups where each agent claims its own API key from a different path.
- Pattern: adapter-level config provides defaults; agent-level config can override any connection parameter.

This decision should be added to the **Key Decisions** section of the existing `adapters/openclaw-gateway/NODE.md` rather than creating a new leaf node.
