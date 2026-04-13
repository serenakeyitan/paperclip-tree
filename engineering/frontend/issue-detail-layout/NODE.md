---
title: "Issue Detail Layout"
owners: [bingran-you, cryppadotta, serenakeyitan]
---

# Issue Detail Layout

Layout and component organization for the issue detail view — how sub-issues, attachments, and navigation elements are arranged within the issue page.

## Key Decisions

### Sub-Issues Rendered Inline

Sub-issues are displayed inline within the parent issue view rather than in a separate tab. Agents and humans navigating issue hierarchies need to see sub-issue status without switching context — the inline display reduces navigation friction for the most common workflow.

### Image Attachments as Gallery Grid

Image attachments are displayed as a square-cropped gallery grid rather than a linear list. This provides a compact visual overview when issues accumulate screenshots or design assets during agent work.

### Scroll-to-Bottom Offset for Properties Panel

The scroll-to-bottom button position accounts for the properties panel width when the panel is open, preventing the button from being obscured.
