---
title: "Issue Detail Layout"
owners: [plind-dm]
---

# Issue Detail Layout

Layout and structural decisions for the issue detail view — the primary surface where agents and humans inspect, edit, and manage individual issues.

## Key Decisions

### Sub-Issues Inline, No Separate Tab

Sub-issues are rendered inline within the issue detail view rather than behind a separate tab. This keeps the full task hierarchy visible without requiring navigation, which is critical for agents that need to see parent-child relationships at a glance.

**Rationale:** Agents frequently need to understand the full decomposition of work when deciding what to do next. A separate tab hides this context behind a click, adding latency to agent decision loops.

### Image Attachments as Square-Cropped Gallery Grid

Image attachments are displayed as a square-cropped gallery grid rather than full-size inline images. This prevents large images from dominating the issue view while still making visual artifacts discoverable.

### Scroll-to-Bottom Button Offset for Properties Panel

The scroll-to-bottom button accounts for the properties panel width when the panel is open, preventing the button from being obscured by the panel overlay.

Source: PR `fix(issues): replace non-null assertions with null checks in checkout…`
