---
title: "MCP Server"
owners: [bingran-you, cryppadotta, serenakeyitan, plind-junior]
---

# MCP Server

Model Context Protocol (MCP) server that exposes Paperclip capabilities as tools for external AI agents and clients. Source: `/mcp-server`.

## Key Decisions

### Tool-Based API Surface

The MCP server exposes Paperclip operations as discrete tools that external agents can invoke. This includes governance operations like approval creation, allowing agents connected via MCP to participate in the approval workflow.

### Strict Request Validation

All incoming MCP API requests are validated tightly — unknown fields are rejected, required fields are enforced, and type constraints are checked at the boundary. This prevents malformed or exploratory requests from reaching business logic.

**Rationale:** MCP clients are untrusted external callers. Loose validation at this boundary would undermine the governance guarantees enforced at the server layer.

### Docker Integration

The MCP server manifest is included in the Docker dependency stage to ensure the MCP server is available in containerized deployments without requiring a full rebuild when only MCP configuration changes.
