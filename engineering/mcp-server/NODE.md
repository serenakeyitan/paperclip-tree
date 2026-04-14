---
title: "MCP Server"
owners: [nya1]
---

# MCP Server

Model Context Protocol (MCP) server that exposes Paperclip capabilities as tools for external AI agents and clients.

Source: `/server/src/mcp/` (or relevant path)

## Key Decisions

### Tool-Based API Surface

The MCP server exposes Paperclip operations as discrete tools (e.g., approval creation) that external agents can invoke via the MCP protocol. This provides a structured, discoverable interface beyond the REST API.

### Strict Request Validation

MCP API requests are validated tightly at the boundary. Invalid or malformed requests are rejected early rather than passed through to service logic.

**Rationale:** MCP clients may be automated agents producing varying request quality. Strict validation at the MCP boundary prevents malformed requests from reaching core services.

### Docker Manifest Inclusion

The MCP server manifest is included in the Docker deps stage to ensure the MCP server is available in containerized deployments.
