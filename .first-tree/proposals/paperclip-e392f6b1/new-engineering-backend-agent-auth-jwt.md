---
type: TREE_MISS
source_id: paperclip-e392f6b1
source_commit_range: db4e1465517f6e96876dda85488d4ab7210412a1..5d1ed71779df5622d9fd99ad28816b2da4bdee31
target_node: new
rationale: The tree documents BetterAuth session-based auth and agent API keys, but does not capture the agent JWT authentication subsystem or the PAPERCLIP_AGENT_JWT_SECRET → BETTER_AUTH_SECRET fallback chain.
---
# Agent JWT Authentication

Agents authenticate to the Paperclip server using JWT tokens signed with a dedicated secret. This is separate from the BetterAuth session-based authentication used by the Board UI.

## Secret Resolution

The agent auth system resolves its JWT signing secret through a fallback chain:

1. **`PAPERCLIP_AGENT_JWT_SECRET`** — dedicated secret for agent JWT signing. Preferred when set.
2. **`BETTER_AUTH_SECRET`** — fallback when the dedicated secret is absent. This allows simpler deployments that only configure one secret.

This is NOT a hardcoded fallback — both values come from environment variables. The "no hardcoded secret fallbacks" security constraint (see `drift/paperclip-e392f6b1/infrastructure/security/NODE.md`) is preserved.

## Key Decisions

### Separate secret for agent JWTs

Agent JWT signing uses its own environment variable (`PAPERCLIP_AGENT_JWT_SECRET`) so that deployments can rotate agent credentials independently of BetterAuth session secrets. The fallback to `BETTER_AUTH_SECRET` exists as a convenience for single-secret deployments (local dev, simple Docker setups) — not as a security mechanism.

**Rationale:** Defense-in-depth. Separate secrets allow independent rotation and reduce blast radius if one secret is compromised. The fallback keeps the onboarding path simple.
