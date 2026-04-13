---
title: "Bind Presets"
owners: [@bingran-you, @cryppadotta, @serenakeyitan]
---

# Bind Presets

Pre-configured network binding profiles for Paperclip deployment, introduced to simplify setup across different network topologies and reduce misconfiguration risk.

**Source context:** PR #3303 (review of OpenClaw networking/discovery/binding patterns).

---

## How It Works

Rather than exposing raw `HOST:PORT` configuration, the deployment setup offers named presets that encode security-appropriate defaults. Operators pick an intent rather than constructing a network address:

- **`localhost`** — Binds only to `127.0.0.1`. Suitable for local development or when behind a reverse proxy on the same host.
- **`tailnet`** — Restricts the server to listen only on the Tailscale interface. Validates that the tailnet interface is present before accepting the configuration, preventing accidental exposure on public or LAN interfaces.
- **`all-interfaces`** — Binds to `0.0.0.0`. For containerized deployments or environments where the network boundary is managed externally (e.g., firewall, security group).

## Key Decisions

### Presets over raw bind addresses

Named presets replace ad-hoc bind address configuration. This was motivated by observing that most deployment failures stemmed from misconfigured bind addresses. Presets encode validated, security-appropriate defaults for each network topology.

### Tailnet bind hardening

The tailnet preset includes a hardening step that validates the Tailscale interface is present before accepting the configuration. This prevents silent fallback to a public interface if Tailscale is not running, which would be a security exposure.
