---
type: TREE_MISS
source_id: paperclip-e392f6b1
source_commit_range: 45ebecab5a3d404970f555d1750dc73cf2b3a2be..5d1ed71779df5622d9fd99ad28816b2da4bdee31
target_node: new
rationale: Issue thread markdown rendering with inline reference resolution (PAP-1234 links) and quicklook-independent rendering is not covered by any existing node — issue-links covers link navigation but not thread display.
---
# Issue Threads

Thread messages on issues render full markdown with inline reference resolution. References like `PAP-1234` are parsed and rendered as navigable links to the referenced issue, agent, or artifact.

## Key Decisions

### Quicklook-Independent Rendering

Thread rendering is decoupled from quicklook routing. Thread messages display identically whether viewed inline, in a detail panel, or via quicklook. This prevents UI regressions where routing context leaks into content rendering.

### Markdown Edge Cases for Agent Content

Agent-generated thread messages often contain code blocks, nested lists, and unusual whitespace. The renderer handles these edge cases to avoid broken display from non-human authorship patterns.
