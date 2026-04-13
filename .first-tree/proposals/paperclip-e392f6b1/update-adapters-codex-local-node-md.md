---
type: TREE_STALE
source_id: paperclip-e392f6b1
source_commit_range: 4d61dbfd34389a4723d6ae3df22df53501b42c71..11de5ae9c9523064a290755c02fb66e4e1c1b1e3
target_node: adapters/codex-local/NODE.md
supersedes: adapters/codex-local/NODE.md
rationale: Fast mode support and missing-rollout error detection were added to the Codex adapter.
---
The existing node should be updated to add:

- **Fast mode** — Codex adapter supports a fast mode flag, but avoids enabling it during env probe to prevent configuration interference.
- **Missing-rollout detection** — The adapter recognizes missing-rollout errors as stale sessions and surfaces them appropriately rather than failing silently.
