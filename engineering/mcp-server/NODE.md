---
title: "MCP Server"
owners: [bingran-you, cryppadotta, serenakeyitan]
---

# MCP Server

Standalone Model Context Protocol server exposing Paperclip capabilities as tools to MCP-compatible AI clients (Claude Desktop, IDE extensions, external agents). Source: `packages/mcp-server`.

Package: `@paperclipai/mcp-server`

## Key Decisions

### Separate Package, Shared Service Logic

The MCP server runs as its own process but imports backend service modules directly rather than calling the REST API. This avoids duplicating business logic while keeping the MCP surface independently deployable. Routes in the MCP server are thin wrappers that translate MCP tool calls into backend service invocations.

### Untrusted Trust Boundary

MCP is treated as an untrusted boundary — input validation at the MCP layer is stricter than internal backend route validation because MCP clients are not authenticated the same way as UI users. Every tool call is scoped to a `company_id`, consistent with the platform-wide isolation model.

### Tool Catalog as API Surface

The MCP manifest defines the complete set of available tools. Unlisted actions are unreachable. The manifest is included in the Docker deps stage for correct layer caching in production container images.

### Current Tools

- **approval-create** — Allows external agents to create approval requests programmatically, bridging MCP into Paperclip's governance layer so agents hitting a governance gate can request sign-off without human intervention in the UI.

## Relationships

- Reuses backend service layer (`engineering/backend/`) — no duplicated business logic
- Integrates with governance approval system (`product/governance/`) — MCP tools go through the same approval workflow as UI and REST API actions
- Packaged in Docker builds (`infrastructure/deployment/`) — manifest included in deps stage
