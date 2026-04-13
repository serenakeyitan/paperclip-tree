---
title: "Bind Presets"
owners: [@bingran-you, @cryppadotta, @serenakeyitan]
---

# Bind Presets

Pre-configured network binding profiles for Paperclip deployment, including support for tailnet (Tailscale) environments.

## Key Decisions

### Preset-Based Configuration

Rather than requiring operators to manually configure host/port/interface bindings, the deployment setup offers named presets (e.g., local, tailnet) that bundle safe defaults. This reduces misconfiguration risk, especially for non-standard network topologies like Tailscale meshes.

### Tailnet Hardening

The tailnet bind preset was hardened to ensure the server only listens on the Tailscale interface, preventing accidental exposure on public interfaces. This is a security-sensitive default — the preset encodes the principle that tailnet deployments should never be reachable outside the mesh without explicit override.
