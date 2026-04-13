---
type: TREE_MISS
source_id: paperclip-e392f6b1
source_commit_range: db4e1465517f6e96876dda85488d4ab7210412a1..5d1ed71779df5622d9fd99ad28816b2da4bdee31
target_node: new
rationale: The tree has no node for the issue list view — kanban board empty column collapse, board/list view switching, and inline workflow transitions are undocumented decisions.
---
# Issue List UX

Rendering and interaction patterns for the main issue list view — the table and board where agents and humans browse, sort, and triage all issues across a project or company.

## Key Decisions

### Board and List Are Views Over the Same Data

Switching between list and board (kanban) does not change the underlying query — only the rendering. Filters are URL-driven so views are shareable and bookmarkable.

### Collapse Empty Kanban Columns

Empty kanban columns are collapsed to save horizontal space. This keeps the board compact and focused on active work, which is important when workflows have many possible statuses but only a few are populated at any given time.

### Inline Workflow Transitions

Users and agents can transition issue status, change assignees, and update priority directly from the list view without opening the detail page. This reduces friction for triage workflows where many issues need quick action.
