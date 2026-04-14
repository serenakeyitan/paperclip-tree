---
title: "MCP Server"
owners: [bingran-you, cryppadotta, serenakeyitan, plind-junior]
---

# MCP Server

Model Context Protocol (MCP) server that exposes Paperclip capabilities as tools for external AI agents and clients. The MCP server is a protocol adapter — it translates MCP tool calls into Paperclip backend service calls.

## Key Decisions

### Approval Creation via MCP

The MCP server exposes an approval creation tool, allowing external agents to programmatically request governance approvals through the MCP protocol. This bridges the governance system (approval gates) with external agent runtimes that connect via MCP rather than the native adapter interface.

### Strict API Request Validation

MCP API requests are validated tightly at the boundary. Invalid or malformed requests are rejected early rather than passed through to backend services. This is a trust boundary — MCP clients may be third-party agents with varying levels of reliability.

### Docker Integration

The MCP server manifest is included in the Docker dependency stage to ensure the MCP server is available in containerized deployments.

## Boundaries

Business logic lives in the backend services, not in the MCP layer. The MCP server only handles protocol translation and request validation.
