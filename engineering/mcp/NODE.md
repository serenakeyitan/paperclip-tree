---
title: "MCP Server"
owners: [bingran-you, cryppadotta, serenakeyitan, plind-junior]
---

# MCP Server

Model Context Protocol (MCP) server that exposes Paperclip capabilities as tools for external AI agents and clients. Source: `/mcp`.

Package: `@paperclipai/mcp`

## Key Decisions

### Approval Creation Tool

The MCP server exposes an approval creation tool, allowing external agents to programmatically create approval requests through the MCP interface. This bridges MCP clients into Paperclip's governance layer — agents connected via MCP can trigger the same approval gates as agents running natively.

### Strict API Request Validation

MCP API requests are tightly validated at the server boundary. Invalid or malformed requests are rejected early rather than passed through to backend services. This is a defense-in-depth measure — MCP is an external-facing protocol surface and must not trust client input.

### Docker Manifest Inclusion

The MCP server manifest is included in the Docker deps stage so that containerized deployments include the MCP surface. This was a bug fix after the manifest was initially omitted, causing MCP tools to be unavailable in Docker-based deployments.
