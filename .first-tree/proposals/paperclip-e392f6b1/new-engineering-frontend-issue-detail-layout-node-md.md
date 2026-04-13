---
type: TREE_MISS
source_id: paperclip-e392f6b1
source_commit_range: db4e1465517f6e96876dda85488d4ab7210412a1..5d1ed71779df5622d9fd99ad28816b2da4bdee31
target_node: new
rationale: PR moves sub-issues inline (removing tabs), adds image attachment gallery grid, and adds properties-panel-aware scroll positioning — layout decisions not captured by thread UX or document freshness nodes.
---
# Issue Detail Layout

Layout and component organization for the issue detail view — how sub-issues, attachments, and navigation elements are arranged within the issue page.

## Key Decisions

### Sub-Issues Rendered Inline

Sub-issues are displayed inline within the parent issue view rather than in a separate tab. This was changed from a tabbed layout because agents and humans navigating issue hierarchies need to see sub-issue status without switching context. The inline display reduces navigation friction for the most common workflow: reviewing a parent issue and its breakdown simultaneously.

### Image Attachments as Gallery Grid

Image attachments are displayed as a square-cropped gallery grid rather than a linear list. This provides a compact visual overview of attached images without consuming excessive vertical space, which is important when issues accumulate screenshots or design assets during agent work.

### Scroll-to-Bottom Offset for Properties Panel

The scroll-to-bottom button position accounts for the properties panel width when the panel is open, preventing the button from being obscured. This is a layout coordination concern between the main content area and the side panel.
