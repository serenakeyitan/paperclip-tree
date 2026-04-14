---
title: "MCP Server"
owners: [bingran-you, cryppadotta, serenakeyitan, googlarz]
---

# MCP Server

Model Context Protocol (MCP) server implementation that exposes Paperclip capabilities as MCP tools, allowing external AI agents to interact with Paperclip programmatically.

## Key Decisions

### MCP as a Tool Surface for Governance Actions

The MCP server exposes governance-sensitive operations (e.g., approval creation) as MCP tools. These tools go through the same server-side validation and authorization as REST API calls — MCP is a transport layer, not a privilege escalation path.

### Strict API Request Validation

MCP tool inputs are validated tightly at the request boundary. Invalid or unexpected parameters are rejected before reaching service logic, preventing malformed agent requests from causing downstream errors.

### MCP Server Manifest in Docker

The MCP server manifest is included in the Docker deps stage so that the production image can serve MCP tool discovery. CI validates that manifest changes are properly included without requiring lockfile changes.

## Boundaries

The MCP server delegates to the same services as the REST API — it does not implement its own business logic. Governance enforcement (approvals, budget checks) is handled by the service layer, not the MCP handler.
