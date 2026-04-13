---
type: TREE_MISS
source_id: paperclip-e392f6b1
source_commit_range: 45ebecab5a3d404970f555d1750dc73cf2b3a2be..5d1ed71779df5622d9fd99ad28816b2da4bdee31
target_node: new
rationale: The tree has no node covering how issue lists are filtered, sorted, grouped, or how inline workflow transitions and refetch optimizations work from the list and board views.
---
# Issue List & Board Workflows

The issue list is the primary surface for browsing, filtering, and acting on issues across a company's task hierarchy. It supports both board-style (kanban) and list-style views.

## Key Patterns

### Inline Workflow Transitions

Users and agents can transition issue status, change assignees, and update priority directly from the list view without opening the detail page. This reduces friction for triage workflows where many issues need quick action.

### Filtering and Grouping

Issue lists support filtering by status, assignee, priority, team, and label. Grouping (e.g., by status column or by assignee) provides different lenses on the same data. These filters map directly to query parameters on the backend issues API.

### Refetch Optimization

Filter-only changes (e.g., toggling a status filter) do not trigger a full data refetch. The frontend distinguishes between filter changes that narrow the existing dataset and changes that require new server data, avoiding unnecessary network requests and UI flicker.

## Key Decisions

- **List and board are views over the same data and API.** Switching between list and board does not change the underlying query — only the rendering.
- **Filters are URL-driven.** Filter state is encoded in the URL so that views are shareable and bookmarkable.
