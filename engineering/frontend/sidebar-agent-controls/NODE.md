---
title: "Sidebar Agent Controls"
owners: [bingran-you, cryppadotta, serenakeyitan]
soft_links: [engineering/backend/heartbeat-run-orchestration/NODE.md, engineering/frontend/NODE.md, product/agent-model/NODE.md, product/governance/NODE.md]
---

Inline controls for managing agents directly from the left sidebar agent list, without navigating to the agent detail page.

## Actions

Each agent row in the sidebar exposes a `MoreHorizontal` dropdown menu with two items:

- **Edit** — links to the agent's edit page via `Link` from `@/lib/router`.
- **Pause / Resume** — a single toggle item whose label flips based on the current agent status. When active, it calls `agentsApi.pause(agentId)`; when paused, it calls `agentsApi.resume(agentId)`. Disabled when `agent.status === "pending_approval"`.

Pause and resume are wired through `useMutation` and invalidate the agents query on success so the sidebar reflects the new state immediately. Failures surface through `useToastActions` (`pushToast`).

### Resume is only offered for manually paused agents

Resume from the sidebar is **disabled** when `agent.pauseReason === "budget"` (the toggle becomes a non-actionable "Budget paused" label, with a `BudgetSidebarMarker` rendered next to the row). This is a governance constraint, not a UI quirk: budget hard-stops are part of the control-plane rules in `product/governance/NODE.md`, and lifting one requires a separate budget action elsewhere — operators cannot bypass a budget-pause through the per-agent control surface. Manual pause and budget pause are therefore distinct states even though they share the underlying `paused` status field, and the sidebar surfaces only the manual case.

## Why inline

Pause/resume is a frequent governance action — operators need to halt a misbehaving agent quickly without losing their place in the current view. Putting it on the sidebar row keeps the action one click away from anywhere in the app and complements the existing live-run indicators (`heartbeatsApi.liveRunsForCompany`) that already render in the same row.

## Testing

`SidebarAgents.test.tsx` covers the dropdown using a jsdom + React Query harness with hoisted mocks for `agentsApi`, `authApi`, `heartbeatsApi`, the router, the toast context, and the sidebar context. New tests should follow the same hoisted-mock pattern rather than spinning up real providers.
