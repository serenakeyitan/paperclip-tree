---
title: "Agent State Visualization"
owners: [@bingran-you, @serenakeyitan, @bingran-you, @serenakeyitan]
---

# Agent State Visualization

How agent lifecycle states (active, paused, idle, running, error, terminated) are communicated visually across frontend surfaces.

## Key Decisions

### Dimmed Treatment for Paused Agents

Paused agents are rendered with reduced opacity in both the agent list view and the org chart view. This provides an at-a-glance distinction between active and paused agents without removing them from the display. Paused agents remain visible and navigable — they are de-emphasized, not hidden.

**Rationale:** In an AI company with many agents, quickly identifying which agents are currently active vs. paused is critical for operators. Hiding paused agents would lose org structure context; a badge-only approach requires reading each row. Opacity dimming works at the peripheral vision level, making it the fastest signal.

### Consistent State Representation Across Views

The same visual treatment applies in both the flat agent list and the hierarchical org chart. This ensures that agent state is legible regardless of which view the user is in, and prevents confusion when switching between views.

## Boundaries

Agent statuses and their lifecycle semantics are defined in [product/agent-model](../../product/agent-model/NODE.md). This node covers only the frontend visual conventions for surfacing those states.
