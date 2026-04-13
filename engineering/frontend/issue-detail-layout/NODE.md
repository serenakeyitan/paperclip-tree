---
title: "Issue Detail Layout"
owners: [bingran-you, cryppadotta, serenakeyitan]
---

# Issue Detail Layout

Layout and rendering decisions for the issue detail view — the full-page surface where agents and humans see an issue's description, attachments, sub-issues, and properties panel.

## Key Decisions

### Sub-Issues Rendered Inline

Sub-issues are displayed inline within the issue detail view rather than in a separate tab. The previous tabbed layout added navigation friction — agents and humans had to switch tabs to see child work. Inline rendering keeps the full task hierarchy visible in a single scroll.

### Image Attachment Gallery Grid

Image attachments are displayed as a square-cropped gallery grid rather than a linear list of full-size previews. This keeps the visual footprint compact when an issue has many screenshots or diagrams, which is common in agent-generated issues that include evidence.

### Scroll-to-Bottom Button Offset

The scroll-to-bottom button is offset when the properties panel is open, preventing it from being obscured. This is a layout-aware positioning decision — the button respects the panel's width so it remains clickable regardless of panel state.
