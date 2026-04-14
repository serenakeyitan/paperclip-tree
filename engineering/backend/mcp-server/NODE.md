---
title: "MCP Server"
owners: [plind-dm]
---

# MCP Server

Model Context Protocol server exposing Paperclip operations as MCP tools for external AI agents and IDEs.

**Source:** `server/src/mcp/`, MCP manifest files.

## Key Decisions

### Tool-Per-Operation Granularity

Each Paperclip operation (e.g., approval creation, issue queries) is exposed as a discrete MCP tool. This keeps tool definitions focused and discoverable by MCP clients.

### Strict API Request Validation

MCP tool inputs are validated tightly at the boundary — malformed or unexpected fields are rejected rather than silently ignored. This prevents downstream bugs from loosely-typed MCP client payloads.

### Manifest in Docker Build

The MCP server manifest is included in the Docker deps stage so that MCP tool metadata is available at container startup without a separate build step.
