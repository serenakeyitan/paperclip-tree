---
type: TREE_MISS
source_id: paperclip-e392f6b1
source_commit_range: a3e125f79659e9d6a2caac8ff3a0eb3cd4127039..d6b06788f6efacb002791c1a60b4889d7bfdb22d
target_node: new
rationale: The PR introduces several UX decisions for issue detail views (inline sub-issues replacing tabs, image attachment gallery grid, scroll-to-bottom offset for properties panel, inline document diff rendering) that represent a coherent set of issue detail conventions not captured in the frontend node.
---
Conventions and decisions for the issue detail view — the primary surface where agents and board members interact with individual issues.

## Key Decisions

### Inline Sub-Issues

Sub-issues are displayed inline on the parent issue rather than in a separate tab. Sub-issues default to the parent's workspace. This keeps context co-located and reduces navigation.

### Image Attachments

Image attachments render as a square-cropped gallery grid rather than a list of links. This makes visual artifacts (screenshots, diagrams, generated images) scannable at a glance.

### Document Diff Rendering

Document diffs are rendered inline within the issue thread rather than requiring navigation to a separate diff view.

### Scroll-to-Bottom Button

The scroll-to-bottom button offsets when the properties panel is open to avoid overlap with the side panel.
