---
title: "Issue Detail UX"
owners: [kimnamu]
---

# Issue Detail UX

Rendering and layout decisions for the issue detail view — the primary surface where agents and humans view and edit individual issues.

## Key Decisions

### Inline Sub-Issues

Sub-issues are displayed inline within the parent issue detail view rather than in a separate tab. This reduces navigation clicks and gives agents immediate visibility into child task status without context-switching.

### Image Attachment Gallery

Image attachments are displayed as a square-cropped gallery grid rather than as a list of links. This provides visual context at a glance, especially useful when agents attach screenshots or diagrams.

### Scroll-to-Bottom Button Offset

The scroll-to-bottom button is offset when the properties panel is open, preventing the button from being obscured by the panel overlay.

### Inline Document Diff Rendering

Document diffs are rendered inline within the issue detail view rather than in a separate diff viewer. This keeps revision comparison contextual to the issue being viewed.
