---
type: TREE_MISS
source_id: paperclip-e392f6b1
source_commit_range: 45ebecab5a3d404970f555d1750dc73cf2b3a2be..5d1ed71779df5622d9fd99ad28816b2da4bdee31
target_node: new
rationale: Codex local adapter gained fast mode support (--fast flag) with an env-probe exclusion, but the existing node has no mention of fast mode.
---
## Fast Mode

The Codex local adapter supports a **fast mode** toggle that passes the `--fast` flag to the Codex CLI, enabling lower-latency responses at the cost of reduced reasoning depth.

### Env Probe Exclusion

Fast mode is explicitly disabled during environment probes. The env probe needs full reasoning to reliably detect tooling and runtime capabilities — fast mode produced unreliable probe results in testing. The adapter checks for probe context before injecting the `--fast` flag.
