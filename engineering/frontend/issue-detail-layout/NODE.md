---
title: "Issue Detail Layout"
owners: [bingran-you, cryppadotta, serenakeyitan]
---

# Issue Detail Layout

Layout and structural decisions for the issue detail view — the primary surface where agents and humans inspect, edit, and act on individual issues.

## Key Decisions

### Sub-issues displayed inline

Sub-issues are rendered inline within the parent issue detail view rather than in a separate tab. The sub-issues tab was removed in favor of this inline display. This reduces navigation friction — agents and humans can see the full issue hierarchy without switching tabs, which is critical for agents that need to understand task decomposition at a glance.

### Image attachments as gallery grid

Image attachments are displayed as a square-cropped gallery grid rather than inline full-width images. This provides a compact visual overview of attached images without dominating the issue detail layout, and scales better when issues have multiple attachments.

### Scroll-to-bottom offset for properties panel

The scroll-to-bottom button position accounts for the properties panel width when it is open, preventing the button from being obscured by the panel overlay.
