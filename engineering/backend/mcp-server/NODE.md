---
title: "MCP Server"
owners: [plind-dm]
---

# MCP Server

Model Context Protocol (MCP) server exposing Paperclip capabilities as tools for external AI agents and clients.

## Key Decisions

### Approval Creation Tool

The MCP server exposes an approval creation tool, allowing external agents to programmatically request human-in-the-loop approval gates through the MCP interface. This bridges the governance system to agents operating outside Paperclip's native adapter layer.

### Strict API Request Validation

All MCP API requests go through tight validation before reaching service logic. This is a trust boundary — MCP clients are external and untrusted, so input validation is strict rather than permissive.

Source: `server/src/` (MCP-related routes and services)
