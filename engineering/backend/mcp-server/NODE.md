---
title: "MCP Server"
owners: [nya1]
---

# MCP Server

Model Context Protocol server that exposes Paperclip capabilities as MCP tools, allowing external AI agents and clients to interact with Paperclip programmatically.

## Key Decisions

### Tool-Based API Surface

The MCP server exposes Paperclip operations as discrete tools (e.g., approval creation, issue management) rather than raw REST proxying. Each tool has typed parameters and validation.

### Strict Request Validation

API requests to the MCP server are validated tightly at the boundary — malformed or unexpected parameters are rejected before reaching business logic. This was tightened after initial deployment revealed overly permissive input handling.

### Server Manifest in Docker Build

The MCP server manifest is included in the Docker dependency stage so that containerized deployments can discover and serve MCP tools without a separate build step.

## Tools

- **Approval creation** — programmatically create approval requests through the governance system
