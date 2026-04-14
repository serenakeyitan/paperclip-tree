---
title: "MCP Server"
owners: [googlarz]
---

# MCP Server

Model Context Protocol server that exposes Paperclip capabilities as MCP tools for external AI agents and clients.

**Source:** `server/src/mcp/` (server-side), `mcp/` (manifest and packaging)

## Key Decisions

### Approval Creation Tool

The MCP server exposes an approval creation tool, allowing external agents to programmatically request governance approvals through the MCP interface. This bridges the governance system with external agent toolchains.

### Strict API Request Validation

MCP API requests are validated tightly — malformed or unexpected fields are rejected rather than silently ignored. This prevents external agents from accidentally invoking tools with incorrect parameters.

### Docker Manifest Inclusion

The MCP server manifest is included in the Docker deps stage to ensure MCP tooling is available in containerized deployments.
