---
title: "Codex Fast Mode"
owners: [@bingran-you, @cryppadotta, @serenakeyitan]
---

# Codex Fast Mode

The Codex Local adapter supports a **fast mode** flag that enables lower-latency responses from the Codex CLI at the cost of reduced reasoning depth.

## Key Decisions

### Fast Mode Suppressed During Environment Probe

Fast mode is explicitly disabled when the adapter runs its environment capability probe (`env probe`). The probe needs full reasoning to reliably detect tooling, runtimes, and workspace state — fast mode's reduced depth produced unreliable probe results. This was discovered during initial integration and hardened as a follow-up fix.

### Activation

Fast mode is passed through as a CLI flag to the spawned `codex` process. It follows the same pattern as other adapter-level capability flags — controlled by the orchestrator, not the agent.
