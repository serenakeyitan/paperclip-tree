---
type: TREE_MISS
source_id: paperclip-e392f6b1
source_commit_range: db4e1465517f6e96876dda85488d4ab7210412a1..5d1ed71779df5622d9fd99ad28816b2da4bdee31
target_node: new
rationale: The PR introduces a secret fallback chain (PAPERCLIP_AGENT_JWT_SECRET → BETTER_AUTH_SECRET) for agent JWT signing that no existing node documents, and the existing Security node only covers the 'no hardcoded fallback' rule without addressing env-var-to-env-var fallback.
---
# Agent JWT Secret Fallback

Agent authentication JWTs are signed with a dedicated secret. The signing secret resolution follows a fallback chain rather than requiring a separate variable in every deployment.

## Decision

When `PAPERCLIP_AGENT_JWT_SECRET` is set, use it as the signing key for agent JWTs. When it is absent, fall back to `BETTER_AUTH_SECRET` — the same secret used by Better Auth for board sessions. This keeps single-secret deployments simple while allowing operators to isolate agent signing in higher-security environments.

## Why

Most deployments already require `BETTER_AUTH_SECRET` for authenticated mode. Requiring a second secret just for agent JWTs adds friction without meaningful security benefit in typical setups, since both secrets protect the same trust boundary (the Paperclip instance). Operators who want defense-in-depth can set `PAPERCLIP_AGENT_JWT_SECRET` to a distinct value so that a compromise of one secret does not automatically grant the other's capabilities.

## Constraints

- This is an env-var-to-env-var fallback, **not** a hardcoded fallback. The "no hardcoded secret fallbacks" rule in the Security node still applies — the server must never start with a baked-in default secret.
- If neither `PAPERCLIP_AGENT_JWT_SECRET` nor `BETTER_AUTH_SECRET` is set, agent JWT signing must fail loudly rather than proceeding with an empty or predictable key.

## Related Domains

- [Security](../security/NODE.md)
- [Deployment](../../infrastructure/deployment/NODE.md)
- [Shared API and Authorization](../../engineering/backend/shared-api-and-actor-scoped-authorization.md)
