---
title: "Bind Presets"
owners: [bingran-you, cryppadotta, serenakeyitan]
---

# Bind Presets

Pre-configured network binding profiles for Paperclip deployment, introduced to simplify setup across different network topologies.

## Key Decisions

### Tailnet-Aware Binding

Bind presets include a tailnet configuration that hardens the network binding setup for overlay networks (e.g., Tailscale). This avoids exposing services on public interfaces when a tailnet is available, and ensures discovery and binding use the correct network identity.

### Preset Model

Rather than requiring users to manually configure host, port, and interface bindings, the deployment setup offers named presets (e.g., `tailnet`, `local`, `public`) that set sensible defaults. This was motivated by observing that most deployment failures stemmed from misconfigured bind addresses.

Source context: PR #3303 (review of OpenClaw networking/discovery/binding patterns).
