---
title: "MCP Server"
owners: [cryppadotta]
---

# MCP Server

Model Context Protocol (MCP) server that exposes Paperclip capabilities as tools for external AI agents and MCP-compatible clients. Source: `/packages/mcp-server`.

## Key Decisions

### Standalone Package

The MCP server is a separate package rather than embedded in the main server. This allows independent versioning and deployment, and keeps the MCP protocol boundary clean.

### Approval Creation Tool

The MCP server exposes an approval creation tool, enabling external agents to programmatically request governance approvals through the MCP protocol. This bridges Paperclip's governance layer with MCP-compatible clients.

### Strict API Request Validation

All incoming MCP tool inputs are validated strictly before forwarding to internal services. Since MCP clients are external and untrusted, this is a critical trust boundary.

### Docker Manifest Inclusion

The MCP server manifest is included in the Docker deps stage, ensuring availability in containerized deployments alongside the main server.

## Boundaries

The MCP server is a thin protocol adapter — it translates MCP tool calls into internal service calls. Business logic remains in the backend services (`engineering/backend/NODE.md`). Governance enforcement (approvals, budget checks) happens at the server layer, not in the MCP adapter (`product/governance/NODE.md`).
