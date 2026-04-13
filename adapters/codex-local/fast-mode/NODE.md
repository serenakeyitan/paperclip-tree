---
title: "Codex Fast Mode"
owners: [@bingran-you, @cryppadotta, @serenakeyitan]
---

# Codex Fast Mode

The Codex local adapter supports a **fast mode** that can be toggled for executions, trading thoroughness for speed.

## Key Decisions

### Fast Mode as an Adapter-Level Toggle

Fast mode is implemented at the adapter layer, passed as a flag when spawning the Codex CLI process. This keeps the decision about execution speed scoped to the adapter rather than requiring server-level orchestration changes.

### Excluded from Environment Probes

Fast mode is explicitly disabled during environment probes (`envTest`). The probe needs reliable, deterministic results to validate that the Codex CLI is installed and authenticated — fast mode's speed optimizations could produce false negatives or incomplete validation.

**Rationale:** Environment probes are a correctness gate. Applying fast mode there would optimize for speed at the cost of reliability, which defeats the purpose of the probe.
