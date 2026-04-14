---
title: "MCP Server"
owners: [bingran-you, cryppadotta, serenakeyitan]
---

# MCP Server

Model Context Protocol server package that exposes Paperclip capabilities as MCP tools for external AI agents. Source: `packages/mcp/`.

Package: `@paperclipai/mcp`

## Key Decisions

### MCP as Trust Boundary

The MCP server is a trust boundary — all incoming requests are strictly validated before reaching backend services. API request validation is tightened to reject malformed or unexpected payloads early. This prevents external agents from bypassing business logic constraints.

### Tool-Based Interface

Paperclip capabilities are exposed as discrete MCP tools (e.g., `approval-create` for programmatic approval requests). Each tool maps to existing backend service functions, keeping the MCP layer thin.

### Shared Backend Services

The MCP server reuses the same service layer as the HTTP API and CLI. It does not duplicate business logic — it translates MCP tool calls into service invocations.

### Docker Manifest Inclusion

The MCP server manifest is included in the Docker deps stage so the production image can serve the tool manifest without a full rebuild when only manifest metadata changes.
