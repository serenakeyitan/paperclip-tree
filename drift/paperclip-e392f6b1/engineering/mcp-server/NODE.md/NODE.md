---
title: "MCP Server"
owners: [@bingran-you, @serenakeyitan, @bingran-you, @serenakeyitan]
---

# MCP Server

Standalone Model Context Protocol (MCP) server that exposes Paperclip capabilities as MCP tools to external AI agents. Source: `packages/mcp/`.

Package: `@paperclipai/mcp`

## Architecture

### Separate Package, Shared Services

The MCP server is a distinct package with its own entry point that imports backend service functions rather than duplicating business logic. This keeps the MCP protocol boundary clean while the backend remains the single source of truth for business rules.

**Rationale:** A single source of truth for business logic prevents drift between the UI-facing REST API and the agent-facing MCP surface.

### Trust Boundary Validation

All incoming MCP requests are strictly validated at the boundary before reaching internal services. MCP callers are external agents with different trust characteristics than authenticated UI users, so validation is tightened independently of backend route validation.

**Rationale:** MCP is a trust boundary. External agents don't go through the same auth flow as the UI. Treating every inbound MCP request as untrusted-by-default keeps the attack surface auditable and the tool catalog explicit.

### Manifest-Based Tool Discovery

The MCP server uses a manifest file to declare available tools. This manifest is included in the Docker deps stage so that layer caching works correctly when only tool definitions change. Tool additions require manifest updates.

## Available Tools

- **approval-create** — Creates approval requests programmatically, enabling external agents to trigger governance workflows via MCP.

## Key Decisions

- MCP tools are read/write but always subject to governance gates (approvals, budget hard-stops).
- The MCP server bridges the governance model with external agent runtimes — adapter-connected agents can invoke MCP tools to participate in governance workflows without direct REST API access.
- CI supports manifest-only changes without requiring lockfile updates.
