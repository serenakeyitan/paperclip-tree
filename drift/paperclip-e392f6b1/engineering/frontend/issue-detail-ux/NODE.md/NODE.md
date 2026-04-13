---
title: "Issue Detail UX"
owners: [@bingran-you, @serenakeyitan, @bingran-you, @serenakeyitan]
---

# Issue Detail UX

UX conventions and layout decisions for the issue detail view.

## Key Decisions

### Inline Sub-Issues

Sub-issues are rendered inline within the parent issue view rather than in a separate tab. This was chosen because agents and humans frequently need to see the full issue context — parent description, attachments, and child work items — in a single scroll. The previous tabbed layout required extra navigation that slowed down triage workflows.

### Image Attachment Gallery

Image attachments are displayed as a square-cropped gallery grid rather than a linear list. This provides a compact visual overview when issues have multiple screenshots or design assets, reducing vertical scroll while keeping images identifiable at a glance.

### Scroll-to-Bottom with Properties Panel Awareness

The scroll-to-bottom button offsets its position when the properties panel is open, preventing overlap with the side panel UI.
