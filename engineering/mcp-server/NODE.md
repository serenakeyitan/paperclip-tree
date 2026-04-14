---
title: "MCP Server"
owners: [bingran-you, cryppadotta, serenakeyitan, plind-junior]
---

# MCP Server

Model Context Protocol (MCP) server that exposes Paperclip capabilities as tools for external AI agents and clients. Source: MCP-related files in `/server`.

## Key Decisions

### Approval Creation via MCP

The MCP server exposes an approval creation tool, allowing external agents to programmatically create approval requests through the MCP interface. This extends the governance model's approval gates to agents connecting via MCP, not just the internal adapter system.

### Strict API Request Validation

MCP API requests are validated tightly at the boundary. Invalid or unexpected fields are rejected rather than silently ignored. This prevents external agents from sending malformed requests that could bypass governance checks or produce unexpected state.

### MCP Manifest in Docker Deps Stage

The MCP server manifest file is included in the Docker deps stage alongside workspace package.json files. This ensures the MCP server configuration is available during dependency installation and Docker layer caching works correctly for MCP-related changes.
