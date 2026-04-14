---
title: "MCP Server"
owners: [plind-dm]
---

# MCP Server

Model Context Protocol server exposing Paperclip capabilities as MCP tools for external AI agents and IDEs.

Source: MCP-related routes and services in `/server/src/`.

## Key Decisions

### Tool-Per-Action Design

Each Paperclip action (e.g., creating an approval, querying issues) is exposed as a discrete MCP tool with strict input validation. This keeps the tool surface explicit and auditable — agents cannot invoke arbitrary server logic through MCP.

### Approval Creation via MCP

External agents can create approval requests through MCP, enabling governance workflows to be triggered from IDE integrations or external orchestrators without going through the web UI.

### Strict API Request Validation

MCP request payloads are tightly validated at the boundary. Malformed or unexpected fields are rejected rather than silently ignored.

### Manifest in Docker Build

The MCP server manifest is included in the Docker dependency stage to ensure tool definitions are available in containerized deployments.
