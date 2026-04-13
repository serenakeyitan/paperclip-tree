---
title: "Bind Presets"
owners: [@bingran-you, @cryppadotta, @serenakeyitan]
---

# Bind Presets

Pre-configured network binding profiles that replace raw bind address configuration with validated, security-appropriate presets.

## Presets

- **localhost** — Binds to `127.0.0.1` only. Default for local development.
- **tailnet** — Binds to the Tailscale interface. Validates the interface is present before accepting configuration.
- **all-interfaces** — Binds to `0.0.0.0`. Intended for containerized deployments behind a reverse proxy.

## Key Decisions

### Tailnet Interface Hardening

The tailnet preset validates that the Tailscale network interface exists and is active before allowing the bind. This prevents silent failures where the server appears to start but is unreachable over the tailnet.

### Presets Over Raw Addresses

Raw bind addresses are error-prone and security-sensitive. Presets encode security intent (e.g., "only expose on tailnet") rather than requiring operators to know the correct IP or interface name.
