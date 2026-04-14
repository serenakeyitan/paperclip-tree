---
title: "MCP Server"
owners: [plind-dm]
---

# MCP Server

Model Context Protocol (MCP) server exposing Paperclip functionality as tools that external AI agents can invoke. Source: `/mcp-server` or equivalent package.

## Key Decisions

### Tool Surface

The MCP server exposes Paperclip domain operations as MCP tools — e.g., approval creation, issue queries. Each tool maps to an existing backend service, keeping the MCP layer thin.

### Validation

API request validation is strict — the MCP server tightens validation on incoming requests to prevent malformed or unexpected payloads from reaching backend services.

### Deployment

The MCP server manifest is included in the Docker deps stage, ensuring it is available in containerized deployments alongside the main server.
