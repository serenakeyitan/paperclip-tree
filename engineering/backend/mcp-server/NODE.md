---
title: "MCP Server"
owners: [plind-dm]
---

# MCP Server

Model Context Protocol server that exposes Paperclip capabilities as MCP tools for external AI agents and clients.

Source: `/mcp-server` (or relevant server path)

## Key Decisions

### Approval Creation Tool

The MCP server exposes an approval creation tool, allowing external agents to programmatically create governance approval requests through the MCP protocol. This bridges the MCP interface with Paperclip's governance layer.

### Strict API Request Validation

MCP API requests are tightly validated at the boundary. Malformed or unexpected payloads are rejected before reaching business logic. This is a defense-in-depth measure given that MCP clients are external and untrusted.

### Manifest Included in Docker Build

The MCP server manifest is included in the Docker dependency stage so that container builds have access to tool definitions without requiring a separate copy step. This was a packaging fix to ensure the MCP server works correctly in containerized deployments.

### CI Support for Manifest-Only Changes

CI pipelines support PRs that change only the MCP manifest without an accompanying lockfile change, preventing false failures on manifest-only updates.
