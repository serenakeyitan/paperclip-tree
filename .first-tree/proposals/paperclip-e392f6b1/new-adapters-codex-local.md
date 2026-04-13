---
type: TREE_MISS
source_id: paperclip-e392f6b1
source_commit_range: 45ebecab5a3d404970f555d1750dc73cf2b3a2be..5d1ed71779df5622d9fd99ad28816b2da4bdee31
target_node: new
rationale: PR #3383 adds fast mode support to the Codex Local adapter, but the existing codex-local node does not document this capability.
---
## Fast Mode

The Codex Local adapter supports **fast mode**, which can be toggled per-task or per-session. When enabled, the adapter passes the appropriate flags to the Codex CLI to use faster inference settings (trading some quality for speed).

This follows the same pattern as other adapter-specific execution modes — the adapter translates Paperclip's abstract execution preferences into CLI-specific flags.

**PR:** [#3383](https://github.com/paperclipai/paperclip/pull/3383)
