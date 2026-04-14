---
title: "MCP Server"
owners: [bingran-you, cryppadotta, serenakeyitan, plind-junior]
---

# MCP Server

The MCP (Model Context Protocol) server exposes Paperclip capabilities as tools that external AI agents can invoke via the standardized MCP protocol. Source: `/mcp`.

Package: `@paperclipai/mcp-server`

## Key Decisions

### Tool Surface Maps to Backend Services

Each MCP tool maps to an existing backend service operation. The MCP layer is a thin protocol adapter — it translates MCP tool invocations into service calls and formats responses back into MCP tool results. No business logic lives in the MCP layer.

### Approval Creation Tool

The MCP server includes a tool for creating approval requests, bridging the governance approval gate system to external agents via MCP. This allows agents connected through MCP to request human approvals without direct API access to the REST API.

### Tight Boundary Validation

MCP API requests are validated strictly at the boundary to prevent malformed or unauthorized tool invocations from reaching backend services. This is the MCP equivalent of the route-level validation in the REST API.

### Docker Manifest Inclusion

The MCP server manifest is included in the Docker deps stage so that containerized deployments expose MCP tools correctly.
