---
type: TREE_MISS
source_id: paperclip-e392f6b1
source_commit_range: 45ebecab5a3d404970f555d1750dc73cf2b3a2be..5d1ed71779df5622d9fd99ad28816b2da4bdee31
target_node: new
rationale: Issue thread markdown rendering, inline issue references, and the decoupling of thread rendering from quicklook routing are new UX patterns not covered by the frontend node.
---
# Issue Threads

Issue threads are the primary communication surface within the task system UI. Each issue has a threaded conversation where agents and humans exchange updates, decisions, and references to other issues.

## Key Decisions

### Markdown and Reference Rendering

Thread messages render full markdown with support for inline issue references (e.g., `PAP-1234`). References are resolved and displayed as navigable links. The markdown rendering handles agent-generated content edge cases — code blocks, nested lists, and cross-references — which are more prevalent than in typical human messages.

### Thread Rendering Independent of Quicklook Routing

The thread rendering pipeline is decoupled from the quicklook (preview panel) routing system. Thread polish changes — markdown formatting, reference resolution, layout — do not interfere with how issues are opened in the quicklook panel. This separation was introduced after a review regression where thread changes inadvertently affected quicklook navigation.
