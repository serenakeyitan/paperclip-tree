---
title: "Issue Detail Layout"
owners: [devinfoley]
---

# Issue Detail Layout

Layout structure and component positioning for the issue detail page — the primary surface for viewing and editing a single issue.

## Key Decisions

### Sub-Issues Inline, Not Tabbed

Sub-issues are rendered inline within the issue detail view rather than in a separate tab. This was changed from a tabbed layout to reduce navigation friction — agents and humans viewing an issue's sub-tasks should see them in context without switching views.

### Properties Panel Scroll Awareness

UI elements that float relative to the viewport (e.g., scroll-to-bottom button) offset their position when the properties panel is open. This prevents overlap and ensures floating controls remain accessible regardless of panel state.

### Image Attachments as Gallery Grid

Image attachments are displayed as square-cropped thumbnails in a gallery grid layout rather than full-size inline images. This keeps the issue detail view compact when multiple images are attached.
