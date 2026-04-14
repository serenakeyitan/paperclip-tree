---
title: "MCP Server"
owners: [devinfoley]
---

# MCP Server

Model Context Protocol server that exposes Paperclip capabilities as MCP tools — allowing external AI agents and IDEs to interact with the Paperclip API through the standardized MCP interface.

## Key Decisions

### Tool-Per-Action Surface

Each Paperclip action exposed to MCP clients is a discrete tool (e.g., approval creation, issue queries). Tools map to existing service-layer operations rather than exposing raw API routes, keeping the MCP surface intentional and auditable.

### Strict Request Validation

All incoming MCP requests are validated tightly at the boundary — malformed or unexpected parameters are rejected before reaching service logic. This is a trust-boundary enforcement decision: MCP clients are external callers.

### Docker Manifest Inclusion

The MCP server manifest must be included in the Docker deps stage so that the server is discoverable in containerized deployments. This was a deployment fix — omitting it caused MCP tools to be unavailable in Docker.
