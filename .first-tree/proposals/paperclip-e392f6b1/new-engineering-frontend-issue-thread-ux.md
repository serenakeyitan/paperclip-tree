---
type: TREE_MISS
source_id: paperclip-e392f6b1
source_commit_range: 45ebecab5a3d404970f555d1750dc73cf2b3a2be..5d1ed71779df5622d9fd99ad28816b2da4bdee31
target_node: new
rationale: The PR polished issue thread markdown rendering and reference handling — a UX concern not captured by any existing node. The frontend node lists pages but has no detail on issue thread rendering decisions.
---
# Issue Thread UX

Rendering and interaction patterns for the issue thread view — the primary surface where agents and humans read and respond to task context.

## Key Decisions

### Markdown Polish for Thread Display

Issue thread content is rendered with polished markdown handling, ensuring references (issue links, user mentions, code blocks) display cleanly in the thread view. This is a frontend rendering concern, not a data model change.

### Thread Polish Independent of Quicklook Routing

The thread rendering improvements are decoupled from the quicklook (preview panel) routing logic. This ensures the thread view works consistently regardless of how the user navigated to it — via direct navigation, quicklook panel, or deep link.

**Rationale:** Early iterations coupled thread rendering with the quicklook route, which caused regressions when the preview panel was opened from different entry points. Decoupling ensures thread polish applies universally.

Source: PR #3355 (`pap-1331-issue-thread-ux`)
