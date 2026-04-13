---
type: TREE_MISS
source_id: paperclip-e392f6b1
source_commit_range: fceefe7f097543bd565309b2290f26ee19191493..b649bd454fce0c5d9aed64e6b75eb302b5d255ba
target_node: new
supersedes: null
rationale: Bind presets for deployment setup and tailnet bind hardening were introduced as new deployment configuration patterns not covered in the deployment node.
---
## Bind Presets and Tailnet

Deployment setup now includes bind presets for common network configurations, with specific hardening for tailnet (Tailscale) setups.

### Features

- **Bind presets** — Pre-configured network binding templates for common deployment scenarios (localhost, LAN, public, tailnet). Simplifies initial deployment configuration.
- **Tailnet hardening** — Tailscale-specific bind setup with security hardening to ensure proper network isolation and authentication.

### Decisions

- Presets over manual configuration — reduces misconfiguration risk for common deployment patterns.
- Tailnet is treated as a first-class deployment target given its prevalence in self-hosted AI infrastructure.
