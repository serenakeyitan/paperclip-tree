---
type: TREE_MISS
source_id: paperclip-e392f6b1
source_commit_range: db4e1465517f6e96876dda85488d4ab7210412a1..5d1ed71779df5622d9fd99ad28816b2da4bdee31
target_node: new
rationale: Multiple commits reshape the issue detail UX (inline sub-issues, image gallery grid, properties panel scroll offset) representing deliberate product decisions about how agents and humans interact with issues — the frontend node only mentions 'detail view' without specifics.
---
# Issue Detail UX

The issue detail view is the primary surface for both humans and agents to inspect, discuss, and act on work items. Source: `/ui/src/` issue-related components.

## Key Decisions

### Inline Sub-Issues

Sub-issues are rendered inline within the parent issue detail view rather than in a separate tab. This reduces navigation friction — agents and users see the full issue hierarchy in one scroll without context-switching between tabs.

### Image Attachment Gallery

Image attachments display as a square-cropped gallery grid rather than a vertical list. This keeps the conversation view compact when agents or users attach multiple screenshots or generated images.

### Properties Panel Scroll Offset

The scroll-to-bottom button accounts for the properties panel width when the panel is open, preventing the button from being obscured. This is a layout coordination pattern — floating UI elements must be aware of panel state.
