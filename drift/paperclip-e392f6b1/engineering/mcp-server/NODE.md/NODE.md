---
title: "MCP Server"
owners: [@bingran-you, @serenakeyitan, @bingran-you, @serenakeyitan]
---

# MCP Server

Model Context Protocol server that exposes Paperclip's capabilities as MCP tools for external AI agents and clients. Source: `/packages/mcp-server`.

Package: `@paperclipai/mcp-server`

## Purpose

Allow any MCP-compatible AI agent (Claude, Cursor, etc.) to interact with Paperclip programmatically — creating issues, reading project state, managing approvals, and more — through the standardized MCP tool interface rather than raw REST calls.

## Key Decisions

### Tool-per-Action Surface

Each Paperclip operation (create issue, list projects, create approval, etc.) is exposed as a discrete MCP tool with its own typed parameter schema. This keeps tool discovery and invocation simple for LLM-based agents.

### Approval Creation Tool

The MCP server includes a tool for creating approval requests, enabling external agents to trigger governance workflows (e.g., hiring approvals, strategy approvals) through the MCP protocol. Approvals created via MCP follow the same server-enforced governance rules as those created through the REST API.

### Request Validation

All incoming MCP requests are validated against strict schemas before reaching service logic. This prevents malformed or unexpected payloads from bypassing business rules.

### Docker Packaging

The MCP server manifest is included in the Docker dependency stage to ensure the server can be deployed as part of the standard Paperclip container image.

## Boundaries

The MCP server is a thin translation layer. It delegates all business logic to the same backend services used by the REST API and UI. Governance, authorization, and audit logging are handled by the server layer, not the MCP server itself.
