---
type: TREE_MISS
source_id: paperclip-e392f6b1
source_commit_range: 45ebecab5a3d404970f555d1750dc73cf2b3a2be..5d1ed71779df5622d9fd99ad28816b2da4bdee31
target_node: new
rationale: PR #3303 introduced bind presets (named network-binding profiles like localhost, tailnet, all-interfaces) and tailnet-aware binding hardening — the existing deployment node covers only Docker stages, compose configs, and env vars with no mention of bind presets or network topology configuration.
---
# Bind Presets

Pre-configured network binding profiles for Paperclip deployment, introduced to simplify setup across different network topologies and reduce misconfiguration risk.

**Source context:** PR #3303 (review of OpenClaw networking/discovery/binding patterns).

## How It Works

Rather than exposing raw `HOST:PORT` configuration, the deployment setup offers named presets that encode security-appropriate defaults:

- **`localhost`** — Binds only to `127.0.0.1`. Suitable for local development or when behind a reverse proxy on the same host.
- **`tailnet`** — Restricts the server to listen only on the Tailscale interface. Validates that the tailnet interface is present before accepting the configuration.
- **`all-interfaces`** — Binds to `0.0.0.0`. For containerized deployments or environments where the network boundary is managed externally.

## Key Decisions

### Presets over raw bind addresses

Named presets replace ad-hoc bind address configuration. Most deployment failures stemmed from misconfigured bind addresses. Presets encode validated, security-appropriate defaults for each network topology.

### Tailnet bind hardening

The tailnet preset validates that the Tailscale interface is actually present before accepting the configuration. If the interface is missing, setup fails immediately rather than binding to a fallback address that could expose the server on an unintended network.
