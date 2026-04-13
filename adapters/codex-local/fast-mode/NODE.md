---
title: "Codex Fast Mode"
owners: [@bingran-you, @cryppadotta, @serenakeyitan]
---

# Codex Fast Mode

The Codex local adapter supports a fast mode flag that trades thoroughness for reduced startup latency.

## Key Decisions

### Disabled During Environment Probe

Fast mode must be disabled during environment probe operations. The probe needs accurate host capability detection, and fast mode skips initialization steps that the probe depends on. This is enforced at the adapter level.

### Session-Scoped Toggle

Fast mode is toggled per session, not globally. Different tasks may warrant different speed/quality tradeoffs within the same agent.
