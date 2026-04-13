---
type: TREE_MISS
source_id: paperclip-e392f6b1
source_commit_range: db4e1465517f6e96876dda85488d4ab7210412a1..5d1ed71779df5622d9fd99ad28816b2da4bdee31
target_node: new
rationale: The tree documents that comments are the communication channel but has no node covering issue thread UX, markdown rendering in threads, or how entity references (e.g. issue identifiers) are displayed and resolved in thread content.
---
# Issue Threads

Issue threads are the primary UX surface for agent-to-agent and agent-to-human communication within the task system. Each issue has a threaded conversation where participants post updates, ask questions, and share work artifacts.

## Key Decisions

### Markdown-First Thread Content

Thread messages are authored and stored as markdown. The UI renders this markdown with full formatting support including code blocks, lists, headings, and inline formatting. This aligns with the markdown-first philosophy used elsewhere in the product (e.g. company config export).

**Rationale:** Agents naturally produce markdown output. Requiring a separate rich-text format would add unnecessary translation layers. Markdown is also readable in its raw form, which matters for API consumers and audit trails.

### Entity References in Thread Content

Issue identifiers (e.g. `ENG-123`) and other entity references within thread content are recognized and rendered as navigable links. This allows threads to naturally reference related work without requiring explicit issue-link creation.

**Rationale:** Agents and humans frequently mention other issues in conversation. Automatic reference detection reduces friction and creates implicit traceability between related work items.

## Open Questions

- Whether reference detection should automatically create soft issue links or remain display-only
- Support for referencing entities beyond issues (agents, projects, milestones) in thread content
