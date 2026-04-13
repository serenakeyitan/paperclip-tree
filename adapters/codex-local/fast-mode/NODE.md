---
title: "Codex Fast Mode"
owners: [@bingran-you, @cryppadotta, @serenakeyitan]
---

# Codex Fast Mode

Support for Codex CLI's fast mode within the Paperclip adapter, allowing agents to execute with reduced latency at the cost of some reasoning depth.

## Key Decisions

### Fast Mode as an Adapter Capability

The Codex local adapter supports passing fast mode flags to the Codex CLI. This is exposed as a configurable option rather than a hardcoded default, allowing per-execution control over the speed/quality tradeoff.

### Excluded from Environment Probes

Fast mode is explicitly disabled during environment probes (CLI validation and auth checks). Environment probes need reliable, deterministic results to correctly assess whether the Codex CLI is installed and authenticated. Fast mode's reduced reasoning could produce incorrect probe results.

**Rationale:** Environment probes are a correctness-critical path — a false negative would prevent an agent from starting. The latency savings of fast mode are irrelevant for a one-time startup check, but the risk of misdetection is real.
