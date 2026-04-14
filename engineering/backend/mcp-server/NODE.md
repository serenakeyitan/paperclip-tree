---
title: "MCP Server"
owners: [plind-dm]
---

# MCP Server

Model Context Protocol (MCP) server that exposes Paperclip capabilities as tools for external AI agents and clients.

Source: `/mcp` (server package)

## Key Decisions

### Tool-Based API Surface

The MCP server exposes Paperclip operations (e.g., approval creation) as MCP tools that external agents can invoke. This complements the REST API with a protocol native to AI agent toolchains.

### Manifest in Docker Deps Stage

The MCP server manifest is included in the Docker deps stage to ensure the MCP package is available in containerized deployments.

### Strict Request Validation

API requests to the MCP server are tightly validated to prevent malformed or unexpected payloads from reaching backend services.
