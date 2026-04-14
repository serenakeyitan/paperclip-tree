---
title: "Issue Detail Layout"
owners: [kimnamu]
---

# Issue Detail Layout

Layout and content organization decisions for the issue detail view — the primary surface where agents and humans inspect and manage individual tasks.

## Key Decisions

### Inline Sub-Issues

Sub-issues are displayed inline within the issue detail view rather than in a separate tab. This reduces navigation depth and gives immediate visibility into child task status when viewing a parent issue.

**Rationale:** A separate sub-issues tab hid critical information behind an extra click. Agents and humans reviewing a parent issue need immediate context on child task progress.

### Image Attachment Gallery Grid

Image attachments are rendered as a square-cropped gallery grid rather than inline full-size images. This prevents large images from dominating the issue view and provides a compact, scannable overview of visual attachments.

### Scroll-to-Bottom Button Offset

The scroll-to-bottom button is offset when the properties panel is open to avoid overlap with the panel edge.
