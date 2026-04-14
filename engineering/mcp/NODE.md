---
title: "MCP Server"
owners: [plind-dm]
---

# MCP Server

Model Context Protocol server that exposes Paperclip capabilities as MCP tools for external AI agents and clients.

## Key Decisions

### Approval Creation Tool

The MCP server exposes an approval creation tool, allowing external agents to programmatically request approvals through the governance system.

### Strict API Request Validation

All MCP API requests are tightly validated at the boundary. This prevents malformed or unexpected payloads from reaching internal services.

### Manifest in Docker Deps Stage

The MCP server manifest is included in the Docker `deps` stage so that container builds have access to the tool definitions at build time.

### CI Support for Manifest-Only Changes

CI pipelines support PRs that change only the MCP manifest without requiring lockfile changes, avoiding false failures on manifest-only updates.
