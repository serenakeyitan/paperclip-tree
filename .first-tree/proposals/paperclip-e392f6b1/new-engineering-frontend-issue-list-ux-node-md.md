---
type: TREE_MISS
source_id: paperclip-e392f6b1
source_commit_range: db4e1465517f6e96876dda85488d4ab7210412a1..5d1ed71779df5622d9fd99ad28816b2da4bdee31
target_node: new
rationale: The PR introduces issue list and board workflow patterns (inline status transitions, filtering, grouping, kanban vs list views) that no existing node covers — the inbox-list node is about the inbox/notification view, not the issue list/board view.
---
# Issue List UX

The issue list is the primary surface for browsing, filtering, and acting on issues across a company's task hierarchy. It supports both board-style (kanban) and list-style views.

## Key Decisions

### Inline Workflow Transitions

Users and agents can transition issue status, change assignees, and update priority directly from the list view without opening the detail page. This reduces friction for triage workflows where many issues need quick action.

### Filtering and Grouping

Issue lists support filtering by status, assignee, priority, team, and label. Grouping (e.g., by status column or by assignee) provides different lenses on the same data.

### List and Board Are Views Over the Same Data

Switching between list and board does not change the underlying query — only the rendering.

### URL-Driven Filters

Filter state is encoded in the URL so that views are shareable and bookmarkable.
