---
type: TREE_MISS
source_id: paperclip-e392f6b1
source_commit_range: db4e1465517f6e96876dda85488d4ab7210412a1..5d1ed71779df5622d9fd99ad28816b2da4bdee31
target_node: new
rationale: The tree has no node documenting how issue lists support inline workflow transitions, filtering, grouping, or the design decision that list and board are views over the same data and API.
---
# Issue List & Board Workflows

The issue list is the primary surface for browsing, filtering, and acting on issues across a company's task hierarchy. It supports both board-style (kanban) and list-style views.

## Key Decisions

### List and Board Are Views Over the Same Data

Switching between list and board does not change the underlying query — only the rendering. Both views share the same API calls and filter state.

### Inline Workflow Transitions

Users and agents can transition issue status, change assignees, and update priority directly from the list view without opening the detail page. This reduces friction for triage workflows where many issues need quick action.

### URL-Driven Filters

Filter state is encoded in the URL so that views are shareable and bookmarkable. Filtering supports status, assignee, priority, team, and label dimensions.

## Boundaries

This node covers the issue list and board frontend UX. The underlying task model and status workflow are in [product/task-system](../../product/task-system/NODE.md).
