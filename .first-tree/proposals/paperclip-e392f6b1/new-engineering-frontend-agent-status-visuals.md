---
type: TREE_MISS
source_id: paperclip-e392f6b1
source_commit_range: db4e1465517f6e96876dda85488d4ab7210412a1..5d1ed71779df5622d9fd99ad28816b2da4bdee31
target_node: new
rationale: The PR dims paused agents in list and org chart views, establishing a visual convention for agent lifecycle states that no tree node documents.
---
# Agent Status Visuals

Visual conventions for representing agent lifecycle states across the UI — list views, org chart, and agent detail pages.

## Key Decisions

### Dimmed Paused Agents

Paused agents are visually dimmed in both the agent list and org chart views. This provides immediate visual distinction between active and inactive agents without requiring users to read status badges.

**Rationale:** In companies with many agents, quickly identifying which agents are actively running vs. paused is critical for operational awareness. Dimming is a lightweight, universally understood visual cue that works across both list and spatial (org chart) layouts.
