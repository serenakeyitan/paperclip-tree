---
title: "MCP Server"
owners: [davison]
---

# MCP Server

Model Context Protocol server exposing Paperclip capabilities as MCP tools for external AI agents and IDEs.

## Key Decisions

### Tool-Per-Action Design

Each Paperclip action (e.g., creating an approval, querying issues) is exposed as a discrete MCP tool with strict input validation. This keeps the tool surface explicit and auditable.

### Approval Creation Tool

External agents can create approval requests through MCP, enabling governance workflows to be triggered from IDE integrations or external orchestrators without going through the web UI.

### Manifest in Docker Build

The MCP server manifest is included in the Docker dependency stage to ensure the MCP tool definitions are available in containerized deployments.

Source: `server/src/` MCP-related routes and services
