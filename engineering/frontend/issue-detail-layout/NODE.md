---
title: "Issue Detail Layout"
owners: [davison]
---

# Issue Detail Layout

Layout and panel composition patterns for the issue detail view — how the properties panel, sub-issues, scroll controls, and content area are arranged.

## Key Decisions

### Sub-Issues Displayed Inline

Sub-issues are rendered inline within the issue detail view rather than in a separate tab. This was changed from a tabbed layout because agents and humans reading an issue need immediate visibility into child work without navigating away from the parent context.

**Rationale:** A separate sub-issues tab hid critical context. Agents walking the task hierarchy need to see parent and children together to make delegation and status decisions.

### Properties Panel Offset

When the properties panel is open, floating UI elements (e.g., scroll-to-bottom button) are offset to avoid overlap. The layout is panel-aware rather than using fixed positioning.

### Image Attachments as Gallery Grid

Image attachments are displayed as a square-cropped gallery grid rather than a linear list. This provides a compact visual overview when issues have multiple screenshots or design assets.

Source: PR "Issue list and issue properties panel: improved UI"
