---
type: TREE_SUPPLEMENT
source_id: paperclip-e392f6b1
source_commit_range: db4e1465517f6e96876dda85488d4ab7210412a1..5d1ed71779df5622d9fd99ad28816b2da4bdee31
target_node: new
rationale: The backend node mentions BetterAuth sessions and agent API keys but does not document the agent JWT authentication mechanism or PAPERCLIP_AGENT_JWT_SECRET configuration, which this PR modifies by adding a fallback to BETTER_AUTH_SECRET.
---
Under **Authentication & Authorization**, add:

### Agent JWT Authentication

Local agents authenticate via short-lived JWTs signed with `PAPERCLIP_AGENT_JWT_SECRET`. When this variable is not set, the server falls back to `BETTER_AUTH_SECRET` as the signing key. This reduces configuration burden for simple deployments while still allowing operators to isolate agent signing keys from session secrets in production.

The `actorMiddleware` resolves agent JWTs into the same request actor model used by board sessions and API keys.
