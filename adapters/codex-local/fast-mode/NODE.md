---
title: "Codex Fast Mode"
owners: [@bingran-you, @cryppadotta, @serenakeyitan]
---

# Codex Fast Mode

The Codex local adapter supports a **fast mode** flag (`--fast`) that reduces startup latency for task execution. When enabled, the adapter skips slower initialization steps to begin work sooner.

## Key Decisions

### Fast mode disabled during environment probe

The adapter's environment-detection step (`env probe`) must run without fast mode. The probe needs the full initialization path to accurately detect the host environment, installed tools, and available capabilities. Running the probe in fast mode would skip checks that inform downstream behavior.

### Opt-in activation

Fast mode is not the default. It is activated when the orchestrator determines that the execution context has already been probed and the environment is known, making the full startup sequence redundant.
