---
type: TREE_MISS
source_id: paperclip-e392f6b1
source_commit_range: fceefe7f097543bd565309b2290f26ee19191493..b649bd454fce0c5d9aed64e6b75eb302b5d255ba
target_node: new
supersedes: null
rationale: Multiple security fixes were merged: removing hardcoded JWT secret fallback, redacting Bearer tokens from logs, bumping multer for CVE fixes, Express 5 wildcard syntax for auth routes, scoped authorization on routes, and webhook signing modes — collectively representing a security posture improvement not captured in the tree.
---
## Security Hardening

Recent security improvements across the backend server addressing authentication, logging, dependencies, and API authorization.

### Changes

- **JWT secret management** — Removed hardcoded JWT secret fallback from `createBetterAuthInstance`. The system now requires explicit `PAPERCLIP_AGENT_JWT_SECRET` or `BETTER_AUTH_SECRET` configuration with proper fallback chain.
- **Log redaction** — Bearer tokens are now redacted from server log output to prevent credential leakage in logs.
- **Dependency CVE fixes** — Multer bumped to 2.1.1 to address HIGH-severity CVEs.
- **Express 5 auth routes** — Auth handler routes updated to Express 5 wildcard syntax for compatibility.
- **Route authorization scoping** — Import, approvals, activity, and heartbeat routes are now properly authorization-scoped to prevent unauthorized access.
- **Webhook signing modes** — Added `github_hmac` and `none` signing modes for webhook verification, supporting multiple webhook source patterns.
- **Node keepAliveTimeout** — Increased behind reverse proxies to prevent 502 errors from premature connection termination.

### Decisions

- No hardcoded secret fallbacks — forces explicit configuration, preventing accidental deployment with weak defaults.
- Authorization is enforced at route level, not middleware-only, ensuring consistent scoping across all endpoints.
