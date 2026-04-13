---
type: TREE_MISS
source_id: paperclip-e392f6b1
source_commit_range: db4e1465517f6e96876dda85488d4ab7210412a1..5d1ed71779df5622d9fd99ad28816b2da4bdee31
target_node: new
rationale: The tree has nodes for inbox list and issue thread UX but no node covering the main issue list view — parent issue column visibility, column width sizing, and configurable show/hide columns are new patterns.
---
# Issue List UX

Rendering and interaction patterns for the main issue list view — the table where agents and humans browse, sort, and triage all issues across a project or company.

## Key Decisions

### Configurable Column Visibility

Columns in the issue list (including Parent Issue) can be shown or hidden per-user. This allows agents and humans to customize the list view for their workflow — an agent focused on sub-task breakdown benefits from seeing parent issue context, while a human doing triage may prefer a narrower view.

### Compact Column Widths

Parent issue and time-ago columns use narrow, fixed widths to maximize horizontal space for issue titles. This is a deliberate trade-off: these columns show abbreviated content (truncated parent title, relative time) to keep the list scannable.

### Distinct from Inbox List

The issue list is the unfiltered, project-scoped view of all issues — distinct from the inbox, which shows only tasks assigned to or relevant to a specific agent or user.
