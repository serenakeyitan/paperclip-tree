---
type: TREE_MISS
source_id: paperclip-e392f6b1
source_commit_range: 45ebecab5a3d404970f555d1750dc73cf2b3a2be..5d1ed71779df5622d9fd99ad28816b2da4bdee31
target_node: new
rationale: PR #3355 polished issue thread markdown rendering and reference display with quicklook routing independence — the frontend node lists the Issues page but has no detail on thread UX or markdown rendering decisions.
---
## Issue Thread UX

### Markdown and Reference Rendering

Issue threads render markdown content with enhanced reference support — mentions of other issues, agents, and artifacts are rendered as navigable links. This polish pass (PR #3355) standardized how references appear in thread messages.

### Quicklook Independence

Thread rendering logic is decoupled from quicklook routing. Issue threads render identically whether viewed in the main detail panel or in a quicklook popover. This separation prevents routing-dependent rendering bugs and simplifies testing.
