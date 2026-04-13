---
title: "MCP Server"
owners: [bingran-you, cryppadotta, serenakeyitan]
---

# MCP Server

Standalone Model Context Protocol server exposing Paperclip capabilities as tools to MCP-compatible AI clients (Claude Desktop, IDE extensions, external agents). Source: `packages/mcp/`.

Package: `@paperclipai/mcp`

## Key Decisions

### Separate Process, Shared Services

The MCP server runs as its own process but imports backend service modules directly rather than calling the REST API. This avoids duplicating business logic while keeping the MCP process independently deployable.

### Untrusted Trust Boundary

MCP is treated as an untrusted boundary — input validation at the MCP layer is stricter than internal backend route validation because MCP clients are not authenticated the same way as UI users. Every tool call is scoped to a `company_id` (platform-wide isolation applies).

### Tool Catalog as API Surface

The MCP manifest defines the complete set of available tools. Unlisted actions are unreachable. The manifest is included in the Docker deps stage for correct layer caching.

### First Tool: Approval Creation

The first shipped MCP tool is `approval-create`, allowing external agents to programmatically request approval on any issue. This bridges MCP into Paperclip's governance layer — agents hitting a governance gate can request sign-off without human intervention in the UI.
