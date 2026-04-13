---
title: "Bind Presets"
owners: [@bingran-you, @cryppadotta, @serenakeyitan]
---

# Bind Presets

Bind presets are predefined network-binding configurations for Paperclip deployment. They control which interfaces and ports the server listens on, replacing ad-hoc bind address configuration with named, validated presets.

## Key Decisions

### Tailnet bind hardening

A dedicated tailnet preset exists for deployments running behind Tailscale. This preset restricts the server to listen only on the Tailscale interface, preventing accidental exposure on public or LAN interfaces. The hardening step validates that the tailnet interface is present before accepting the configuration.

### Presets over raw bind addresses

Rather than exposing raw `HOST:PORT` configuration, the system uses named presets (e.g., `localhost`, `tailnet`, `all-interfaces`) that encode security-appropriate defaults. This reduces misconfiguration risk — operators pick an intent rather than constructing a network address.
