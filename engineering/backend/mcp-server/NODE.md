---
title: "MCP Server"
owners: [plind-dm]
---

# MCP Server

Model Context Protocol server exposing Paperclip capabilities as MCP tools for external AI agents and IDEs.

## Key Decisions

### Approval Creation via MCP

The MCP server exposes an approval creation tool, allowing external agents to programmatically request governance approvals through the MCP interface. This bridges the governance gate system (approvals, budget controls) to agents that connect via MCP rather than the native adapter protocol.

### Strict API Request Validation

MCP API requests are validated tightly at the boundary — malformed or unexpected fields are rejected rather than silently ignored. This prevents external tools from accidentally triggering unintended behavior through the MCP surface.

### Docker Manifest Inclusion

The MCP server manifest is included in the Docker dependency stage to ensure the MCP tool definitions are available at container build time, not just at runtime.
