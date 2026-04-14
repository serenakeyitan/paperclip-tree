---
title: "MCP Server"
owners: [bingran-you, cryppadotta, serenakeyitan, devinfoley]
---

# MCP Server

Model Context Protocol server that exposes Paperclip capabilities as MCP tools for external AI agents and clients. Source: `/mcp` or relevant server routes.

## Key Decisions

### Tool-Based API Surface

Paperclip functionality is exposed through MCP tools rather than requiring direct API integration. This allows any MCP-compatible agent to interact with the Paperclip instance — creating approvals, querying tasks, and managing resources through the standardized MCP protocol.

### Approval Creation Tool

The MCP server includes a tool for creating approval requests, enabling external agents to trigger governance workflows without direct API access. This keeps the approval gate enforceable even when agents interact through MCP rather than the native API.

### Strict Request Validation

API requests through the MCP server are tightly validated to prevent malformed or unauthorized operations. Validation is applied at the MCP layer before requests reach backend services.

### Deployment Packaging

The MCP server manifest is included in the Docker deps stage to ensure the MCP server is available in containerized deployments without requiring a separate build step.
