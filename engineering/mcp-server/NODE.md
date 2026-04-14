---
title: "MCP Server"
owners: [plind-dm]
---

# MCP Server

Model Context Protocol (MCP) server that exposes Paperclip capabilities as tools for external AI agents and IDEs.

**Source:** `packages/mcp-server/`
**Package:** `@paperclipai/mcp-server`

## Key Decisions

### Thin Adapter Over Backend Services

The MCP server is a protocol adapter — it translates MCP tool calls into backend service calls. It does not implement business logic. This keeps the MCP surface consistent with the REST API and board UI.

### Approval Creation Tool

The MCP server exposes an approval creation tool, allowing external agents to programmatically request governance approvals through the MCP interface. This extends the approval gate system beyond the REST API and board UI.

### Strict API Request Validation

All MCP tool inputs are validated with tight schemas. Invalid requests are rejected early rather than passed through to backend services.

**Rationale:** MCP tools are invoked by external AI agents that may produce malformed inputs. Strict validation at the MCP boundary prevents downstream errors and potential security issues.

## Deployment

The MCP server manifest is included in the Docker dependency stage to ensure it is available at runtime.
