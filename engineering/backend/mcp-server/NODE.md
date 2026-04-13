---
title: "MCP Server"
owners: [bingran-you, cryppadotta, serenakeyitan]
---

# MCP Server

The Model Context Protocol server exposes Paperclip capabilities as MCP tools that external agents and clients can invoke. Source: `server/src/mcp/`, manifest in the package root.

## Key Decisions

### Tool-per-Capability Surface

Each discrete Paperclip action (e.g., creating an approval, querying issues) is exposed as a separate MCP tool with its own schema. This keeps tool descriptions narrow and discoverable — an MCP client can list available tools without parsing a monolithic API surface.

### Strict Request Validation

All incoming MCP requests are validated against the tool's declared input schema before reaching service logic. This was tightened after discovering that loosely validated requests could pass unexpected fields through to backend services.

### Docker Manifest Inclusion

The MCP server manifest is explicitly included in the Docker image's dependency stage so that containerized deployments advertise the correct tool surface. Previously the manifest was missing from the deps layer, causing MCP clients to see an empty tool list on fresh container starts.
