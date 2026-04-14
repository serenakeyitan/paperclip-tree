---
title: "MCP Server"
owners: [plind-dm]
---

# MCP Server

Model Context Protocol server exposing Paperclip capabilities as MCP tools for external AI agents and IDEs. Source: `packages/mcp-server`.

## Key Decisions

### Thin Boundary Layer

The MCP server is a thin integration surface that delegates all business logic to existing backend services. It does not duplicate service logic — it validates requests at the MCP boundary and forwards to the same service layer used by the REST API.

### Strict Request Validation

MCP API requests are validated tightly at the boundary — malformed or unexpected fields are rejected rather than silently ignored. External MCP callers are untrusted, so validation is stricter than internal service-to-service calls.

### Approval Creation Tool

The MCP server exposes an approval creation tool, allowing external agents to programmatically request governance approvals through the MCP interface. This bridges the governance gate system to agents connecting via MCP rather than the native adapter protocol.

### Company-Scoped Access Control

All MCP tool calls are scoped to a company, consistent with the platform-wide company isolation model.

### Docker Manifest Inclusion

The MCP server manifest is included in the Docker dependency stage to ensure tool definitions are available at container build time.
