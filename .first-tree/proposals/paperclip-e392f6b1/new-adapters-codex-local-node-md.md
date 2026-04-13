---
type: TREE_MISS
source_id: paperclip-e392f6b1
source_commit_range: 45ebecab5a3d404970f555d1750dc73cf2b3a2be..5d1ed71779df5622d9fd99ad28816b2da4bdee31
target_node: new
rationale: PR adds fast mode support to the Codex adapter (--fast flag, env probe exclusion) which is not mentioned in the existing codex-local node.
---
Update the existing Capabilities section to add:

- **Fast mode:** Supports `--fast` flag for accelerated responses. Fast mode is explicitly excluded during environment probe (`envTest`) to avoid skewing capability detection results.

Update the Key Decisions section to add:

### Fast Mode Exclusion in Environment Probe

The `--fast` flag is not passed during `envTest` (environment capability probing). Fast mode changes model behavior in ways that could produce different capability signals, so probing always uses the standard mode to get consistent results.
