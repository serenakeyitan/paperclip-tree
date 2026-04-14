---
title: "MCP Server"
owners: [cryppadotta]
---

# MCP Server

Model Context Protocol server exposing Paperclip capabilities as tools for external AI agents. Source: `/server/src/mcp/`.

## Key Decisions

### Tool Manifest Discovery

MCP tools are declared in a manifest file included in the Docker build. The manifest drives tool registration and client-side discovery.

### Strict API Request Validation

All incoming MCP requests are validated tightly at the boundary — the server rejects malformed or unexpected payloads before they reach business logic. This is a trust boundary: external agents are untrusted callers.

### Approval Creation Tool

External agents can programmatically create approval requests through the MCP interface, integrating with the governance approval gate system.

### Docker Manifest Inclusion

The MCP server manifest is included in the Docker deps stage to ensure tool discovery works in containerized deployments.

## Boundaries

The MCP server exposes a distinct protocol surface but delegates to existing backend services (approvals, issues, etc.) rather than implementing its own business logic.
