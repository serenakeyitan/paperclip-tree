---
title: "MCP Server"
owners: [bingran-you, cryppadotta, serenakeyitan, plind-junior]
---

# MCP Server

The MCP (Model Context Protocol) server exposes Paperclip capabilities as tools that external AI agents can invoke. Source: `/server/src/mcp/` and related manifests.

## Key Decisions

### Tool Surface

The MCP server provides a standardized tool interface for external agents to interact with Paperclip's task system and governance layer. This includes an approval creation tool that bridges the governance approval gate system to MCP-connected agents, allowing them to request human approvals without direct REST API access.

### Strict Request Validation

MCP API requests are validated tightly at the boundary. Malformed or unauthorized tool invocations are rejected before reaching backend services. This is a trust boundary — MCP clients are treated as external callers.

### Docker Manifest Inclusion

The MCP server manifest is included in the Docker deps stage to ensure MCP tooling is available in containerized deployments.
