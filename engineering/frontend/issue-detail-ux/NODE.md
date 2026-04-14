---
title: "Issue Detail UX"
owners: [devinfoley]
---

# Issue Detail UX

Layout and interaction patterns for the issue detail view — the primary workspace where humans and agents collaborate on individual tasks.

## Key Decisions

### Inline Sub-Issues

Sub-issues are displayed inline within the parent issue view rather than in a separate tab. This reduces navigation friction and keeps the full issue hierarchy visible in context.

### Image Attachment Gallery Grid

Image attachments render as square-cropped gallery grids rather than inline full-size images, keeping the thread scannable when agents attach screenshots or diagrams.

### Scroll-to-Bottom Button Offset

The scroll-to-bottom button offsets its position when the properties panel is open, preventing overlap with side panels.

### Inline Document Diff Rendering

Document diffs are rendered inline within the thread rather than in a separate diff view, keeping context visible without navigation.
