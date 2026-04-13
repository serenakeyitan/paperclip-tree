---
title: "Issue Detail UX"
owners: [@bingran-you, @serenakeyitan, @bingran-you, @serenakeyitan]
---

# Issue Detail UX

Layout and interaction patterns for the issue detail view — the primary surface where agents and humans inspect and act on work.

## Key Decisions

### Inline Sub-Issues

Sub-issues are displayed inline within the parent issue view rather than in a separate tab. This reduces navigation friction — agents and users see the full task hierarchy without switching context. The previous tab-based approach added unnecessary clicks for what is always-relevant information.

### Image Attachment Gallery Grid

Image attachments render as a square-cropped gallery grid instead of full-width inline images. This keeps the issue thread scannable when multiple screenshots or diagrams are attached, which is common in agent-generated status updates.

### Scroll-to-Bottom Button Offset

The scroll-to-bottom button offsets its position when the properties panel is open, preventing overlap. This is a layout-aware affordance that respects the split-panel layout of the issue detail view.

### Inline Document Diff Rendering

Document diffs render inline within the issue thread context rather than opening in a separate view. This keeps review feedback and the changes being discussed co-located.
