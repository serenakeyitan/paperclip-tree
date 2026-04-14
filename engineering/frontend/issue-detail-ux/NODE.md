---
title: "Issue Detail UX"
owners: [plind-dm]
---

# Issue Detail UX

Layout and interaction patterns for the issue detail view — the full-page surface where agents and humans inspect, edit, and manage individual issues.

## Key Decisions

### Sub-Issues Rendered Inline

Sub-issues are displayed inline within the parent issue detail view rather than in a separate tab. This reduces navigation friction — agents and humans can see the full task decomposition without switching contexts.

**Rationale:** A separate sub-issues tab hid critical context behind a click. Agents triaging work need to see the parent issue's description and its sub-issue breakdown in one view.

### Image Attachments as Gallery Grid

Image attachments are displayed as a square-cropped gallery grid rather than a vertical list. This provides a compact visual overview of attached images without dominating the issue detail layout.

### Scroll-to-Bottom Offset for Properties Panel

The scroll-to-bottom button is offset when the properties panel is open so it does not overlap the panel. This is a layout coordination concern between the thread view and the side panel.

### Document Diff Rendered Inline

Document diffs are rendered inline within the issue detail rather than in a separate view or modal. This keeps revision comparisons in context with the rest of the issue.
