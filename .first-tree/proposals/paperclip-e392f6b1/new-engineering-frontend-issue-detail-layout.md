---
type: TREE_MISS
source_id: paperclip-e392f6b1
source_commit_range: db4e1465517f6e96876dda85488d4ab7210412a1..5d1ed71779df5622d9fd99ad28816b2da4bdee31
target_node: new
rationale: Moving sub-issues inline and removing the sub-issues tab (c414790) is a significant UX pattern change to the issue detail view that no existing node covers — existing issue-thread-ux nodes cover thread rendering, not layout structure.
---
# Issue Detail Layout

Layout and information hierarchy decisions for the issue detail view — the primary surface where agents and humans inspect a single issue's full context.

## Key Decisions

### Sub-Issues Displayed Inline

Sub-issues are rendered inline within the parent issue view rather than behind a separate tab. This eliminates a navigation step for agents and humans who need to see the complete task decomposition at a glance. Agents frequently need to understand the full issue hierarchy to make execution decisions, and tab-based navigation added friction to that workflow.

### Scroll-to-Bottom Button Respects Panel State

The scroll-to-bottom button offsets its position when the properties side panel is open, preventing it from being obscured. Layout elements must remain accessible regardless of which auxiliary panels are visible.
