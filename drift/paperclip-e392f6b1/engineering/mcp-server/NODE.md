---
title: "MCP Server"
owners: [@bingran-you, @serenakeyitan, @bingran-you, @serenakeyitan]
---

# MCP Server

Model Context Protocol server exposing Paperclip operations as MCP tools for external AI agents and IDEs. Source: `/packages/mcp-server`.

Package: `@paperclipai/mcp-server`

## Architecture

### Manifest-Driven Tool Discovery

Available tools are declared in a manifest file. The manifest is included in Docker's dependency stage so that tool definition changes trigger proper layer cache invalidation without requiring a full rebuild.

### Shared Business Logic

The MCP server imports backend service functions directly rather than duplicating logic or calling the REST API. This ensures a single source of truth for business rules while treating MCP as a distinct trust boundary with its own input validation layer.

### Strict Input Validation

All MCP tool inputs are tightly validated before forwarding to backend services. MCP is treated as an external trust boundary — validation is independent of and in addition to any REST API validation, since MCP callers may not go through the same authentication path as UI users.

## Key Decisions

- **MCP as a first-class integration surface** — not a wrapper around REST. External agents interact with Paperclip through typed MCP tools rather than raw HTTP calls.
- **Governance tools are MCP-accessible** — approval creation (and future governance operations) can be triggered programmatically by external agents, bridging MCP to the governance system.
- **Separate trust boundary** — MCP inputs are validated independently from REST API inputs, even when both call the same service functions.
