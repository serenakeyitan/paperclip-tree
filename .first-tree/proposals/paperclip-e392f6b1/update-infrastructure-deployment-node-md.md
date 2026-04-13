---
type: TREE_STALE
source_id: paperclip-e392f6b1
source_commit_range: 4d61dbfd34389a4723d6ae3df22df53501b42c71..11de5ae9c9523064a290755c02fb66e4e1c1b1e3
target_node: infrastructure/deployment/NODE.md
supersedes: infrastructure/deployment/NODE.md
rationale: Bind presets for deployment setup, tailnet bind hardening, and keepAliveTimeout tuning for reverse proxies were added — new deployment concerns.
---
The existing node should be updated to reflect:

- **Bind presets** — Deployment setup now supports bind presets for common networking configurations, reducing manual bind configuration.
- **Tailnet bind hardening** — Tailnet-based deployments have hardened bind setup to prevent misconfiguration.
- **Reverse proxy keepAliveTimeout** — Node's `keepAliveTimeout` is increased behind reverse proxies to prevent premature 502 errors from proxy/Node timeout mismatches.
