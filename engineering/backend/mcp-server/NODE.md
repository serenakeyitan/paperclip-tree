---
title: "MCP Server"
owners: [cryppadotta]
---

# MCP Server

Model Context Protocol (MCP) server exposing Paperclip capabilities as MCP tools for external AI agents and clients.

## Key Decisions

### Approval Creation Tool

The MCP server exposes an approval creation tool, allowing external agents to programmatically request human-in-the-loop approvals through the MCP protocol. This extends the governance model beyond Paperclip's own agent adapters.

### Strict API Request Validation

All MCP API requests are tightly validated before processing. This is a security-critical boundary — MCP clients are external and untrusted, so input validation is stricter than internal service-to-service calls.

### Docker Manifest Inclusion

The MCP server manifest is included in the Docker dependency stage, ensuring the MCP server is available in production container images alongside the main server.

## Boundaries

The MCP server lives in the backend codebase but exposes a distinct protocol surface. It delegates to existing backend services (approvals, issues, etc.) rather than implementing its own business logic.
