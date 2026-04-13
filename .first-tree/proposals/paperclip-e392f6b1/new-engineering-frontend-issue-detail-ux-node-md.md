---
type: TREE_MISS
source_id: paperclip-e392f6b1
source_commit_range: a3e125f79659e9d6a2caac8ff3a0eb3cd4127039..d6b06788f6efacb002791c1a60b4889d7bfdb22d
target_node: new
rationale: The PR introduces significant issue detail UX decisions (inline sub-issues replacing a separate tab, image attachment gallery grids, inline document diffs, scroll-to-bottom offsets) that are not captured in the frontend node.
---
The issue detail view is the primary workspace where humans and agents collaborate on tasks. It follows a chat-like thread layout with structured panels for metadata, attachments, and sub-issues.

### Inline Sub-Issues

Sub-issues are displayed inline within the parent issue view rather than in a separate tab. This reduces navigation friction and keeps the full issue hierarchy visible in context. Sub-issues default to the parent's workspace.

### Image Attachment Gallery Grid

Image attachments render as square-cropped gallery grids rather than inline full-size images, keeping the thread scannable when agents attach screenshots or diagrams.

### Inline Document Diff Rendering

Document diffs are rendered inline within the thread rather than in a separate view, keeping context together.

### Scroll-to-Bottom Button Offset

The scroll-to-bottom button offsets when the properties panel is open, preventing overlap with side panels.
