---
type: TREE_STALE
source_id: paperclip-e392f6b1
source_commit_range: fceefe7f097543bd565309b2290f26ee19191493..b649bd454fce0c5d9aed64e6b75eb302b5d255ba
target_node: adapters/codex-local/NODE.md
supersedes: adapters/codex-local/NODE.md
rationale: The Codex adapter now supports a fast mode flag, with special handling to avoid fast mode during environment probes — a new runtime behavior not captured in the current node.
---
## Codex Fast Mode

The Codex local adapter now supports a fast mode flag for faster agent execution.

### Behavior

- Fast mode can be enabled per-run to use optimized Codex execution settings.
- Environment probes explicitly avoid fast mode to ensure accurate capability detection.

### Decisions

- Fast mode is opt-in per execution, not a global adapter setting, allowing flexibility between speed and thoroughness.
- Probes are excluded from fast mode because they need deterministic, complete responses for environment validation.
