---
title: "Issue Workspace Runtime MCP Tools"
owners: [bingran-you, cryppadotta, serenakeyitan]
soft_links: ["engineering/mcp-server/NODE.md", "engineering/backend/NODE.md", "product/task-system/NODE.md"]
---

# Issue Workspace Runtime MCP Tools

The MCP server exposes tools for inspecting and controlling the runtime services attached to an issue workspace, so agents can start, wait on, and query long-running services (e.g. dev servers) without leaving the MCP interface.

## Key Decisions

### Read Tools

- `paperclipGetIssueWorkspaceRuntime` returns the current runtime state of an issue workspace (which services are declared, their current status, last transition).
- `paperclipWaitForIssueWorkspaceService` blocks until a named service reaches a ready state, so agents can sequence actions that depend on a service being up (e.g. wait for a dev server before running smoke tests).

### Write Tool

`paperclipControlIssueWorkspaceServices` mutates service state (start / stop / restart) for the issue workspace. It is the single control-plane entry point for workspace services via MCP — other mutations flow through existing issue, comment, or approval tools.

### Scope

These tools are scoped to a specific issue workspace and respect the same company/actor authorization as other MCP issue tools. They do not replace the HTTP or CLI control-plane paths; they expose the same backend service layer to MCP clients.

## Relationships

- Extends the MCP tool surface defined in the parent [mcp-server/NODE.md](../NODE.md).
- Backed by the workspace runtime services in `engineering/backend/` (issue checkout + service lifecycle).
- Intended for agent runtimes that drive long-running issue workspaces — see `product/task-system/` for the issue-workspace lifecycle.

## Source

Introduced in [paperclipai/paperclip#4223 — "Harden heartbeat scheduling and runtime controls"](https://github.com/paperclipai/paperclip/pull/4223).
