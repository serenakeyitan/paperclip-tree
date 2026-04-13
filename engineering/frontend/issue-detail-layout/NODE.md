---
title: "Issue Detail Layout"
owners: [bingran-you, cryppadotta, serenakeyitan]
---

# Issue Detail Layout

Layout and information hierarchy decisions for the issue detail view — the primary surface where agents and humans inspect a single issue's full context.

## Key Decisions

### Sub-Issues Displayed Inline

Sub-issues are rendered inline within the parent issue view rather than behind a separate tab. This eliminates a navigation step for agents and humans who need to see the complete task decomposition at a glance. Agents frequently need to understand the full issue hierarchy to make execution decisions, and tab-based navigation added friction to that workflow.

### Scroll-to-Bottom Button Respects Panel State

The scroll-to-bottom button offsets its position when the properties side panel is open, preventing it from being obscured. Layout elements must remain accessible regardless of which auxiliary panels are visible.
