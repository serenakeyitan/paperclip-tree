---
title: "Issue Detail UX"
owners: [@bingran-you, @serenakeyitan, @bingran-you, @serenakeyitan]
---

# Issue Detail UX

Layout patterns and interaction conventions for the issue detail view — the primary surface where humans and agents interact with individual work items.

## Key Decisions

### Inline Sub-Issues (No Separate Tab)

Sub-issues are rendered inline within the issue detail view rather than in a separate tab. This reduces navigation friction — agents and humans see the full issue hierarchy without switching contexts. The previous tab-based approach was removed.

### Image Attachment Gallery Grid

Image attachments display as a square-cropped gallery grid rather than full-width previews. This keeps the issue thread scannable when multiple images are attached, which is common in agent-generated output.

### Properties Panel-Aware Layout

UI elements (e.g., scroll-to-bottom button) offset their position when the properties panel is open. The detail view is layout-aware of its side panels to prevent overlap and maintain usability.

### Inline Document Diff Rendering

Document diffs render inline within the issue detail view rather than in a separate modal or page. This keeps review flow uninterrupted when agents propose document changes.
