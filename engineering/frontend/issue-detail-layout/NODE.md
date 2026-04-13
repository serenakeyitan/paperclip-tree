---
title: "Issue Detail Layout"
owners: [bingran-you, cryppadotta, serenakeyitan]
---

# Issue Detail Layout

Layout and rendering patterns for the issue detail view — the primary surface where agents and humans inspect, edit, and manage individual issues.

## Key Decisions

### Image Attachments as Gallery Grid

Image attachments render as square-cropped gallery grids rather than inline full-size images. This keeps the thread scannable when agents attach multiple screenshots or diagrams, providing a compact visual overview without consuming excessive vertical space.

**Rationale:** Agents frequently attach screenshots and generated images. Full-size inline rendering made threads unreadable when multiple images were present. The gallery grid balances visibility with scannability.

### Sub-Issues Rendered Inline

Sub-issues are displayed inline within the parent issue view rather than in a separate tab. The sub-issues tab has been removed. This puts the full issue hierarchy — parent context and child breakdown — on a single scrollable surface.

**Rationale:** Agents and humans reviewing a parent issue need to see both the issue context and its decomposition simultaneously. A separate tab forced context switching and made it harder to reason about the relationship between parent and children.

### Scroll-to-Bottom Button Offset

The scroll-to-bottom button position accounts for the properties panel width when open, preventing the button from being obscured by side panels.
