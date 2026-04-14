---
title: "MCP Server"
owners: [plind-dm]
---

# MCP Server

Model Context Protocol (MCP) server that exposes Paperclip capabilities as tools for external AI agents and MCP-compatible clients. Source: `/packages/mcp-server`.

## Key Decisions

### Standalone Package

The MCP server is a separate package rather than embedded in the main server. This allows independent versioning and deployment, and keeps the MCP protocol boundary clean.

### Approval Creation Tool

The MCP server exposes an approval creation tool, enabling external agents to programmatically request governance approvals through the MCP protocol. This bridges Paperclip's governance layer with MCP-compatible clients.

### Strict Request Validation

API request validation is tightened at the MCP boundary — inputs are validated before being forwarded to backend services. This prevents malformed requests from reaching internal services and provides clear error messages to MCP clients.
