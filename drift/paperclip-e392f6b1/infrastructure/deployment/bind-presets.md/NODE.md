---
title: "Bind Presets and Tailnet"
owners: []
---

## Bind Presets and Tailnet

Deployment setup now includes bind presets for common network configurations, with specific hardening for tailnet (Tailscale) setups.

### Features

- **Bind presets** — Pre-configured network binding templates for common deployment scenarios (localhost, LAN, public, tailnet). Simplifies initial deployment configuration.
- **Tailnet hardening** — Tailscale-specific bind setup with security hardening to ensure proper network isolation and authentication.

### Decisions

- Presets over manual configuration — reduces misconfiguration risk for common deployment patterns.
- Tailnet is treated as a first-class deployment target given its prevalence in self-hosted AI infrastructure.
