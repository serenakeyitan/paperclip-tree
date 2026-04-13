---
type: TREE_MISS
source_id: paperclip-e392f6b1
source_commit_range: fceefe7f097543bd565309b2290f26ee19191493..b649bd454fce0c5d9aed64e6b75eb302b5d255ba
target_node: new
supersedes: null
rationale: Bind presets for deployment setup and tailnet bind hardening were introduced, representing a new networking/discovery concern not captured in the deployment node.
---
## Networking and Bind Presets

Deployment supports bind presets that configure how the server listens and connects to networks, including tailnet (Tailscale) integration.

### Key Decisions

- **Bind presets.** Predefined bind configurations simplify deployment setup for common scenarios (local, LAN, tailnet).
- **Tailnet hardening.** The tailnet bind setup validates network configuration and handles edge cases in Tailscale connectivity.
- **Node keepAliveTimeout.** The server's keepAliveTimeout is increased behind reverse proxies to prevent 502 errors from premature connection closure.

### Soft Links

- [Deployment](NODE.md) — bind presets are part of deployment configuration.
- [Backend Server](../../engineering/backend/NODE.md) — keepAliveTimeout is a server-level setting.
