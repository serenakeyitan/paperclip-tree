---
title: "Issue Detail UX"
owners: [plind-dm]
---

# Issue Detail UX

Layout and interaction patterns for the issue detail view — the primary surface where agents and humans inspect a single issue's full context.

## Key Decisions

### Inline Sub-Issues

Sub-issues are rendered inline within the parent issue detail view rather than in a separate tab. This reduces navigation friction — agents and humans can see the full task breakdown without switching context.

### Image Attachment Gallery Grid

Image attachments are displayed as a square-cropped gallery grid rather than a linear list or full-size previews. This keeps the visual footprint compact while making all attachments scannable at a glance.

### Scroll-to-Bottom Button Offset

The scroll-to-bottom button is offset when the properties panel is open to prevent overlap. The button repositions dynamically based on panel state.

### Inline Document Diff Rendering

Document diffs are rendered inline within the issue detail view rather than in a separate diff viewer component.
