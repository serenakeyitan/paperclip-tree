---
title: "MCP Server"
owners: [@bingran-you, @serenakeyitan, @bingran-you, @serenakeyitan]
---

# MCP Server

Model Context Protocol server that exposes Paperclip capabilities as MCP tools for external AI agents. Source: `/mcp`.

Package: `@paperclipai/mcp-server`

## Purpose

Allow external AI agents (Claude, Codex, etc.) to interact with a Paperclip instance through the standard MCP tool protocol. The server translates MCP tool calls into Paperclip API requests, handling authentication, validation, and error mapping.

## Key Decisions

### Approval Creation Tool

The MCP server exposes an `approval-create` tool that lets agents programmatically request human-in-the-loop approval gates. This extends the governance system beyond the web UI and CLI, enabling agents to trigger approval workflows from any MCP-compatible client.

### Strict Request Validation

All incoming MCP tool calls are validated against tight schemas before forwarding to the Paperclip API. This prevents malformed or overly broad requests from reaching backend services.

### Docker Packaging

The MCP server manifest is included in the Docker deps stage so the server can be deployed alongside the main Paperclip instance without a separate build step.
