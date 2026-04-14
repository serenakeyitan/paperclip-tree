---
title: "MCP Server"
owners: [davison]
---

# MCP Server

Model Context Protocol (MCP) server exposing Paperclip capabilities as tools that external AI agents can invoke.

## Key Decisions

### Approval Creation Tool

The MCP server exposes a tool for creating approvals, allowing external agents to trigger governance gates programmatically through the MCP interface.

### Strict Request Validation

All MCP API requests go through tightened validation to reject malformed or unexpected payloads early. This prevents downstream errors and hardens the MCP surface against misuse.

### Docker Manifest Inclusion

The MCP server manifest is included in the Docker deps stage to ensure the MCP tool definitions are available at container build time, not just at runtime.
