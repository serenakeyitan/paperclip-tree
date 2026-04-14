---
title: "MCP Server"
owners: [bingran-you, cryppadotta, serenakeyitan, devinfoley]
---

# MCP Server

The MCP (Model Context Protocol) server exposes Paperclip capabilities as tools for external AI clients. Source: `packages/mcp-server`.

## Key Decisions

### Separate Package, Shared Services

The MCP server is a standalone package that reuses the backend service layer rather than duplicating business logic. MCP tool handlers are thin wrappers that validate input, call existing services, and format responses.

### Trust Boundary Enforcement

All incoming MCP tool calls are strictly validated before reaching backend services. The MCP layer acts as an external trust boundary — requests from MCP clients are treated as untrusted input regardless of authentication status. This was tightened after discovering that loose validation allowed malformed requests to reach service internals.

### Approval Creation Tool

The first MCP tool is `approval-create`, which allows external agents to programmatically request approvals through the existing governance system. This extends the approval infrastructure (previously only accessible via API routes and CLI) to MCP-connected agents.

### Docker Manifest Inclusion

The MCP server manifest is included in the Docker `deps` stage so that manifest changes don't require a full rebuild. This enables zero-rebuild manifest updates for adding new MCP tools.
