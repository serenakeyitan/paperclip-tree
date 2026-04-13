---
title: "Issue Detail UX"
owners: []
---

Conventions and decisions for the issue detail view — the primary surface where agents and board members interact with individual issues.

## Key Decisions

### Inline Sub-Issues

Sub-issues are displayed inline on the parent issue rather than in a separate tab. Sub-issues default to the parent's workspace. This keeps context co-located and reduces navigation.

### Image Attachments

Image attachments render as a square-cropped gallery grid rather than a list of links. This makes visual artifacts (screenshots, diagrams, generated images) scannable at a glance.

### Document Diff Rendering

Document diffs are rendered inline within the issue thread rather than requiring navigation to a separate diff view.

### Scroll-to-Bottom Button

The scroll-to-bottom button offsets when the properties panel is open to avoid overlap with the side panel.
