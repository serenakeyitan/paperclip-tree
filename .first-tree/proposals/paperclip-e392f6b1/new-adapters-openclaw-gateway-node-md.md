---
type: TREE_MISS
source_id: paperclip-e392f6b1
source_commit_range: a3e125f79659e9d6a2caac8ff3a0eb3cd4127039..d6b06788f6efacb002791c1a60b4889d7bfdb22d
target_node: new
rationale: The OpenClaw gateway node documents auth modes and session strategies but does not capture the per-agent configuration pattern (e.g., claimedApiKeyPath) that allows individual agents to override gateway-level defaults.
---
## Per-Agent Configuration Overrides

Gateway-level configuration (auth tokens, API key paths, session strategies) can be overridden on a per-agent basis. This allows a single gateway to serve multiple agents with different credentials and routing.

- **`claimedApiKeyPath`** is configurable per agent, not just at the adapter level. This supports multi-tenant gateway setups where each agent claims its own API key from a different path.
- Pattern: adapter-level config provides defaults; agent-level config can override any connection parameter.

This decision should be added to the **Key Decisions** section of the existing `adapters/openclaw-gateway/NODE.md` rather than creating a new leaf node.
