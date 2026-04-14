---
title: "Issue Detail Layout"
owners: [plind-dm]
---

# Issue Detail Layout

Layout and panel structure for the issue detail view — the primary surface where agents and humans inspect, edit, and act on individual issues.

## Key Decisions

### Sub-Issues Displayed Inline

Sub-issues are rendered inline within the issue detail view rather than in a separate tab. This removes the tab-based navigation pattern for sub-issues and surfaces the full issue hierarchy in a single scrollable view.

**Rationale:** Agents and humans frequently need to see both the parent issue context and its sub-issues simultaneously. A separate tab hid sub-issues behind a click, breaking the visual connection between parent and child work items. Inline display keeps the full task decomposition visible at a glance.

### Scroll-to-Bottom Button Offset for Properties Panel

The scroll-to-bottom button is offset when the properties panel is open, preventing it from being obscured by the panel overlay.

Source: PRs `Move sub-issues inline and remove sub-issues tab`, `Offset scroll-to-bottom button when properties panel is open`
