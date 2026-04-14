---
title: "MCP Server"
owners: [bingran-you, cryppadotta, serenakeyitan]
---

# MCP Server

Model Context Protocol (MCP) server that exposes Paperclip capabilities as tools for external AI agents and clients. Source: MCP-related server routes and manifest.

## Key Decisions

### Approval Creation Tool

The MCP server exposes an approval creation tool, allowing external agents to programmatically create approval requests through the MCP interface. This extends the governance approval gates (defined in `product/governance/`) to MCP clients.

### Strict API Request Validation

MCP API requests are tightly validated — malformed or unexpected fields are rejected rather than silently ignored. This is a defense-in-depth measure since MCP clients may be untrusted or semi-trusted external agents.

### Manifest in Docker Deps Stage

The MCP server manifest is included in the Docker deps stage so that the MCP surface is available in production containers. This was an explicit fix — omitting the manifest caused MCP tools to be unavailable in Docker deployments.
