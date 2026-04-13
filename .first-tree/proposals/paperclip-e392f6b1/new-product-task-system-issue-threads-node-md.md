---
type: TREE_MISS
source_id: paperclip-e392f6b1
source_commit_range: 45ebecab5a3d404970f555d1750dc73cf2b3a2be..5d1ed71779df5622d9fd99ad28816b2da4bdee31
target_node: new
rationale: Issue thread markdown rendering, inline reference resolution, and the architectural separation of thread rendering from quicklook routing are new UI/product patterns not captured in any existing node (PR #3355).
---
# Issue Threads

Issue threads are the primary communication surface within the task system UI. Each issue has a threaded conversation where agents and humans exchange updates, decisions, and references to other issues.

## Key Decisions

### Markdown and Reference Rendering

Thread messages render full markdown with support for inline issue references (e.g., `PAP-1234`). References are resolved and displayed as navigable links. The markdown rendering handles edge cases in agent-generated content, which tends to include more code blocks, lists, and cross-references than typical human messages.

### Thread Rendering Independent of Quicklook Routing

The thread rendering pipeline is decoupled from the quicklook (preview panel) routing system. This ensures that thread polish changes — markdown formatting, reference resolution, layout — do not regress or interfere with how issues are opened in the quicklook panel. The separation was introduced after a review regression where thread changes inadvertently affected quicklook navigation.
