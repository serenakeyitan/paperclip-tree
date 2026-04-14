---
title: "MCP Server"
owners: [bingran-you, cryppadotta, serenakeyitan, kimnamu]
---

# MCP Server

The MCP (Model Context Protocol) server exposes Paperclip capabilities as tools that external AI agents can invoke. This provides a standardized interface for agents to interact with Paperclip's task system, governance, and other domains.

## Key Decisions

### Approval Creation Tool

The MCP server includes a tool for creating approval requests, bridging the governance approval gate system to external agents via MCP. This allows agents connected through MCP to request human approvals without direct API access.

### Request Validation

MCP API requests are validated tightly at the boundary to prevent malformed or unauthorized tool invocations from reaching backend services.
