---
supersedes: adapters/codex-local/NODE.md
source_commit: b649bd454fce0c5d9aed64e6b75eb302b5d255ba
---

## Codex Fast Mode

The Codex local adapter now supports a fast mode flag for faster agent execution.

### Behavior

- Fast mode can be enabled per-run to use optimized Codex execution settings.
- Environment probes explicitly avoid fast mode to ensure accurate capability detection.

### Decisions

- Fast mode is opt-in per execution, not a global adapter setting, allowing flexibility between speed and thoroughness.
- Probes are excluded from fast mode because they need deterministic, complete responses for environment validation.
