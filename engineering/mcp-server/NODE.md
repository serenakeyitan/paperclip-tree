---
title: "MCP Server"
owners: [cryppadotta]
---

# MCP Server

Model Context Protocol (MCP) server that exposes Paperclip capabilities as tools for external AI agents and clients.

## Key Decisions

### Approval Creation Tool

The MCP server exposes an approval creation tool, enabling external agents to programmatically request governance approvals through the MCP protocol. This bridges Paperclip's governance layer with MCP-compatible clients.

### Strict API Request Validation

All incoming MCP API requests are validated tightly. This prevents malformed or unexpected payloads from reaching internal services — a critical boundary since MCP clients are external and untrusted.

### Docker Manifest Inclusion

The MCP server manifest is included in the Docker deps stage, ensuring the MCP server is available in containerized deployments alongside the main server.

## Boundaries

The MCP server sits between external MCP clients and the backend server's service layer. It reuses existing services (approvals, issues, etc.) rather than reimplementing business logic. Governance enforcement still happens at the server layer per `product/governance/NODE.md`.
