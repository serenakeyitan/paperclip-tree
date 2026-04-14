---
title: "MCP Server"
owners: [bingran-you, cryppadotta, serenakeyitan, plind-junior]
---

# MCP Server

Model Context Protocol server that exposes Paperclip capabilities as MCP tools, allowing external AI agents to interact with the Paperclip control plane programmatically.

## Key Decisions

### Approval Creation via MCP

The MCP server exposes an approval creation tool, enabling external agents to programmatically request governance approvals through the MCP protocol. This extends the governance system's reach beyond the REST API and board UI.

### Strict API Request Validation

All MCP API requests are validated tightly at the boundary. Invalid or unexpected fields are rejected rather than silently ignored. This prevents external agents from accidentally relying on undocumented behavior.

### Docker Manifest Inclusion

The MCP server manifest is included in the Docker deps stage so that MCP tooling is available in containerized deployments. This was a gap that caused MCP tools to be missing in Docker-based installations.

## Open Questions

- Which additional Paperclip operations should be exposed as MCP tools?
- Authentication model for MCP connections vs REST API keys
