---
title: "Sidebar Agent Controls"
owners: [bingran-you, cryppadotta, serenakeyitan]
soft_links: [engineering/backend/heartbeat-run-orchestration/NODE.md, engineering/frontend/NODE.md, product/agent-model/NODE.md]
---

Inline controls for managing agents directly from the left sidebar agent list, without navigating to the agent detail page.

## Actions

Each agent row in the sidebar exposes a `MoreHorizontal` dropdown menu with three actions:

- **Pause** — calls `agentsApi.pause(agentId)`. Hidden when the agent is already paused.
- **Resume** — calls `agentsApi.resume(agentId)`. Hidden when the agent is active.
- **Edit** — links to the agent's edit page via `Link` from `@/lib/router`.

Pause and resume are wired through `useMutation` and invalidate the agents query on success so the sidebar reflects the new state immediately. Failures surface through `useToastActions` (`pushToast`).

## Why inline

Pause/resume is a frequent governance action — operators need to halt a misbehaving agent quickly without losing their place in the current view. Putting it on the sidebar row keeps the action one click away from anywhere in the app and complements the existing live-run indicators (`heartbeatsApi.liveRunsForCompany`) that already render in the same row.

## Testing

`SidebarAgents.test.tsx` covers the dropdown using a jsdom + React Query harness with hoisted mocks for `agentsApi`, `authApi`, `heartbeatsApi`, the router, the toast context, and the sidebar context. New tests should follow the same hoisted-mock pattern rather than spinning up real providers.
