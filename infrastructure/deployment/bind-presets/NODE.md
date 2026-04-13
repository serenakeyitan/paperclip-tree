---
title: "Bind Presets"
owners: [@bingran-you, @cryppadotta, @serenakeyitan]
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

The tailnet preset validates the Tailscale interface is present before accepting the configuration. This prevents silent fallback to a public interface if Tailscale is not running.
