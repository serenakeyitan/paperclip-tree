---
type: TREE_MISS
source_id: paperclip-e392f6b1
source_commit_range: db4e1465517f6e96876dda85488d4ab7210412a1..5d1ed71779df5622d9fd99ad28816b2da4bdee31
target_node: new
rationale: Bearer token redaction from server logs is a new security convention not captured anywhere in the tree; the existing security proposal only covers secret fallback prevention.
---
# Security

Security conventions and constraints for the Paperclip platform.

## Key Decisions

### No Hardcoded Secret Fallbacks

Auth initialization must fail loudly at startup if `BETTER_AUTH_SECRET` is missing — never fall back to a hardcoded value. This is defense-in-depth independent of Docker Compose enforcement. A missing secret is a deployment error that must be caught immediately, not papered over with a default that could be exploited.

### Bearer Token Redaction in Logs

Bearer tokens are redacted from all server log output. Authorization headers and token values must never appear in plaintext in logs, regardless of log level. This prevents credential leakage through log aggregation systems, error reports, or debug output. The redaction is applied at the logging layer so individual route handlers cannot accidentally bypass it.

### Defense in Depth

Security controls are layered — secrets validation at startup, token redaction at the log layer, forbidden-token checks in CI (`scripts/check-forbidden-tokens.mjs`), capability-dropped Docker containers for untrusted review, and governance enforcement at the server layer. No single layer is assumed sufficient.
