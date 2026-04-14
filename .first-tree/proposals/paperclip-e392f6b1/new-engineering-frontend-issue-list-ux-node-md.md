---
type: TREE_MISS
source_id: paperclip-e392f6b1
source_commit_range: 5e65bb2b92ae765815b6816cef60c25cdda837ca..7f893ac4ec9f700efaf902be8a57ce510c1c7092
target_node: new
rationale: The tree has no dedicated issue-list UX node — the issue list is a distinct surface from the inbox list and from issue threads, with its own layout and interaction patterns.
---
# Issue List UX

Rendering and interaction patterns for the issue list view — the primary surface where agents and humans browse, filter, and select issues within a project or company context.

## Key Decisions

### Issue List Distinct from Inbox

The issue list view is a separate surface from the inbox list. While the inbox shows notifications and assigned tasks across all contexts, the issue list shows all issues within a specific project or scope. UX improvements to the issue list are scoped independently from inbox UX.

### Information Density and Interaction Improvements

The issue list received layout, information density, and interaction pattern improvements to support efficient browsing by both agents and humans.
