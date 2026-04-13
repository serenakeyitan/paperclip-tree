---
title: "Bind Presets"
owners: [@bingran-you, @cryppadotta, @serenakeyitan]
---

# Bind Presets

Configurable network binding presets that control how the Paperclip server listens for connections. Introduced alongside tailnet bind hardening (PR #3303, informed by OpenClaw's networking and discovery model).

## Key Decisions

### Preset-Based Bind Configuration

Rather than requiring operators to manually specify host/port combinations, Paperclip offers named bind presets that encode common deployment topologies. This reduces misconfiguration risk, especially for tailnet and private-network setups.

### Tailnet Bind Hardening

When binding to a tailnet (e.g., Tailscale), the setup validates reachability and enforces stricter defaults. This prevents accidental exposure of the Paperclip server to the public internet when the intent is private tailnet-only access.

## Boundaries

- Bind presets control the server's listen address and network exposure. They do not manage firewall rules, DNS, or TLS termination.
- Tailnet integration assumes the tailnet daemon is already running on the host. Paperclip does not manage tailnet authentication or node enrollment.
