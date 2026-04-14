---
type: TREE_MISS
source_id: paperclip-e392f6b1
source_commit_range: db4e1465517f6e96876dda85488d4ab7210412a1..5d1ed71779df5622d9fd99ad28816b2da4bdee31
target_node: new
rationale: The main tree has no infrastructure/security node; the drift proposal exists but hasn't been merged into the main tree, and this PR solidifies the 'no hardcoded JWT secret fallback' security convention as a load-bearing decision.
---
# Security

Security conventions, hardening decisions, and defense-in-depth constraints for Paperclip infrastructure and application code.

## Key Decisions

### No hardcoded secret fallbacks

Authentication initialization must never fall back to a hardcoded secret when the required `BETTER_AUTH_SECRET` environment variable is missing. The server must fail loudly at startup rather than silently operating with a predictable secret. This applies at the application layer — Docker compose also enforces the variable is set, but the server code must independently refuse to start without it.

**Rationale:** Defense-in-depth. Relying solely on the deployment layer (compose `?:` syntax) to enforce secret presence leaves a gap when the server is run outside Docker (e.g., direct `node` invocation, CI, or dev scripts). A hardcoded fallback would allow the server to start in authenticated mode with a known secret, which is a critical security vulnerability.

### Secret handling conventions

- Agent API keys are hashed at rest (plaintext shown once at creation).
- Secrets are redacted in logs and API responses.
- The untrusted-review container drops ALL Linux capabilities and prevents privilege escalation.

See also: [deployment/NODE.md](../deployment/NODE.md) for container-level security hardening and [engineering/backend/NODE.md](../../engineering/backend/NODE.md) for the secrets provider abstraction.
