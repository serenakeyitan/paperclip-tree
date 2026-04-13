---
type: TREE_MISS
source_id: paperclip-e392f6b1
source_commit_range: 45ebecab5a3d404970f555d1750dc73cf2b3a2be..5d1ed71779df5622d9fd99ad28816b2da4bdee31
target_node: new
rationale: The codex-local adapter node documents capabilities but has no mention of fast mode, a new execution flag that must be disabled during env probing (commits 2d8f97f, a7dc889).
---
# Codex Fast Mode

The Codex local adapter supports a **fast mode** flag that reduces startup latency for task execution. When enabled, the adapter skips slower initialization steps to begin work sooner.

## Key Decisions

### Fast mode disabled during environment probe

The adapter's environment-detection step (`env probe`) must run without fast mode. The probe needs the full initialization path to accurately detect the host environment, installed tools, and available capabilities. Running the probe in fast mode would skip checks that inform downstream behavior.

### Opt-in activation

Fast mode is not the default. It is activated when the orchestrator determines that the execution context has already been probed and the environment is known, making the full startup sequence redundant.
