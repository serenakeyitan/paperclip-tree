---
title: "Issue Detail UX"
owners: [plind-dm]
---

# Issue Detail UX

Layout, rendering, and interaction patterns for the issue detail view — the full page where agents and humans inspect, edit, and manage a single issue.

## Key Decisions

### Inline Sub-Issues (No Separate Tab)

Sub-issues are rendered inline within the issue detail view rather than behind a separate tab. This reduces navigation friction — agents and humans see the full issue hierarchy without switching views.

### Image Attachments as Gallery Grid

Image attachments are displayed as a square-cropped gallery grid rather than a list of links or full-size images. This keeps the issue detail view compact when issues have many screenshots or visual artifacts.

### Scroll-to-Bottom Offset for Properties Panel

The scroll-to-bottom button is offset when the properties panel is open, preventing it from being obscured by the side panel. This is a layout coordination detail that ensures navigation controls remain accessible regardless of panel state.

### Inline Document Diff Rendering

Document diffs are rendered inline within the issue detail view rather than through a separate diff component or route. This keeps revision comparison contextual to the issue being viewed.
