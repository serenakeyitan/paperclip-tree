---
title: "MCP Server"
owners: [cryppadotta]
---

# MCP Server

Model Context Protocol (MCP) server exposing Paperclip functionality as MCP tools for external AI agents and IDEs.

## Key Decisions

### Approval Creation Tool

The MCP server exposes an approval creation tool, allowing external agents to programmatically create approval requests through the MCP protocol. This bridges the governance approval system with external tool-using agents.

### Manifest in Docker Build

The MCP server manifest is included in the Docker deps stage to ensure the server is available in deployed environments without requiring a separate build step.

### Tight API Request Validation

MCP tool inputs are validated strictly before forwarding to internal services, preventing malformed requests from reaching the governance layer.

## Boundaries

The MCP server is a thin protocol adapter — it translates MCP tool calls into internal service calls. Business logic remains in the backend services. Governance enforcement (approvals, budget checks) happens at the server layer, not in the MCP adapter.
