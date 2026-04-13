---
type: TREE_MISS
source_id: paperclip-e392f6b1
source_commit_range: db4e1465517f6e96876dda85488d4ab7210412a1..5d1ed71779df5622d9fd99ad28816b2da4bdee31
target_node: new
rationale: PR #3383 adds fast mode support to the Codex local adapter, a new capability not mentioned in the existing Codex Local Adapter node.
---
# Codex Fast Mode

The Codex local adapter supports a **fast mode** that optimizes for speed over thoroughness. When enabled, Codex runs with reduced reasoning overhead for quicker task completion.

**Source:** `packages/adapters/codex-local/`

## Behavior

- Fast mode is passed to the Codex CLI process at spawn time.
- This mirrors similar fast/turbo mode patterns in other adapters (e.g., Claude Code's `--fast` flag).
- The mode selection is exposed through the adapter's session parameters, allowing Paperclip's task orchestration to choose speed vs. thoroughness per task.

## Decision Context

Fast mode was added to give Paperclip companies finer-grained control over agent behavior — some tasks (e.g., simple file edits, status checks) benefit from speed, while complex reasoning tasks should use the default mode. This aligns with Paperclip's governance-first approach: budget-conscious companies can route lower-stakes tasks through fast mode to reduce token spend.
