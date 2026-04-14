---
title: "MCP Server"
owners: [devinfoley]
---

# MCP Server

Model Context Protocol server that exposes Paperclip capabilities as MCP tools, enabling external AI agents and clients to interact with Paperclip programmatically.

## Key Decisions

### Approval Creation Tool

The MCP server exposes a tool for creating approval requests, allowing MCP-connected agents to trigger governance gates (e.g., hiring approvals) without going through the REST API directly.

### Strict API Request Validation

All incoming MCP requests are validated tightly at the server boundary to prevent malformed or unexpected payloads from reaching backend services.

### Manifest-Driven Tool Discovery

The MCP server uses a manifest file to declare available tools. This manifest is included in the Docker build's dependency stage to ensure tool availability in production deployments.

## Boundaries

The MCP server is a transport layer — it translates MCP protocol calls into Paperclip service calls. Business logic stays in the backend services. Governance enforcement stays server-side per the governance model.
