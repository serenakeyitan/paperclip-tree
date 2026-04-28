---
title: "Manual Workspace Runtime Control"
owners: [bingran-you, cryppadotta, serenakeyitan]
soft_links: [engineering/backend/heartbeat-run-orchestration/NODE.md, engineering/execution-workspaces/NODE.md, engineering/mcp-server/issue-workspace-runtime-tools/NODE.md]
---

# Manual Workspace Runtime Control

Workspace runtime services support a `manual` desired state in addition to `running` and `stopped`. Manual is an operator-owned mode that signals "Paperclip should not touch this service" — neither automated agent runs nor API-driven start/stop actions on the workspace as a whole will alter a service flagged as manual.

## Key Decisions

### Manual Is a First-Class Desired State

`WorkspaceRuntimeDesiredState` is `running | stopped | manual`. Both per-workspace `desiredState` and per-service entries in `serviceStates` accept `manual`. Validators on execution-workspace and project workspace runtime configs (`packages/shared/src/validators/`) accept the new value, and persistence/snapshot code in the heartbeat and execution-workspace services round-trips it.

### Agent Runs Skip Manual Services

`ensureRuntimeServicesForRun` returns no started services when the workspace's `desiredState` is `manual`. Agent runs therefore do not auto-start runtime services in manual workspaces, leaving full control to the operator.

### Targeted API Actions Respect Manual

When `buildWorkspaceRuntimeDesiredStatePatch` applies a start/stop action, services already marked `manual` are intentionally left untouched even if the action targets that service index. Workspace-wide fallback also preserves manual entries instead of coercing them to running/stopped.

## Why This Exists

Some services (databases, long-running dev environments, externally managed processes) must not be cycled by Paperclip's automation. Manual mode gives operators an explicit opt-out that survives agent runs, route handlers, and config snapshots without requiring them to remove the service from the workspace runtime config.
