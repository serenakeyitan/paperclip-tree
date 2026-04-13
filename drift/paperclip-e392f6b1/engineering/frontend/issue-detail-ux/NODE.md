---
title: "Issue Detail UX"
owners: [@bingran-you, @serenakeyitan, @bingran-you, @serenakeyitan]
---

# Issue Detail UX

UX conventions for the issue detail view — the primary workspace where humans and agents collaborate on tasks.

## Key Decisions

### Inline Sub-Issues

Sub-issues display inline on the parent issue rather than in a separate tab. This keeps the full task hierarchy visible in context and reduces navigation friction for both humans and agents reviewing work breakdown.

### Image Attachment Gallery Grid

Image attachments render as square-cropped gallery grids rather than full-size inline images. This keeps the conversation thread scannable when agents attach multiple screenshots or diagrams, preventing images from dominating the viewport.

### Scroll-to-Bottom Button Offset

The scroll-to-bottom button offsets horizontally when the properties panel is open, preventing overlap with the side panel and maintaining accessibility regardless of panel state.

### Inline Document Diff Rendering

Document diffs render within the conversation thread rather than requiring navigation to a separate diff view. Changes stay in context with the conversation that produced them, supporting the agent-centric workflow where document edits are interleaved with discussion.
