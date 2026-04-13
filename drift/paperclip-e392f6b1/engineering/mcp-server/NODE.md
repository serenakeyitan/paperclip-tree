---
title: "MCP Server"
owners: [@bingran-you, @serenakeyitan, @bingran-you, @serenakeyitan]
---

# MCP Server

Model Context Protocol server exposing Paperclip capabilities as MCP tools for external AI agents. Source: `/packages/mcp`.

Package: `@paperclipai/mcp`

## Purpose

The MCP server is Paperclip's integration surface for agents that speak the Model Context Protocol. Rather than requiring agents to use the REST API directly, MCP provides a tool-based interface that agents can discover and invoke natively.

## Key Decisions

### Manifest-Driven Tool Discovery

Available tools are declared in a manifest file. The Docker build includes this manifest in the dependency stage so tool metadata is available without a full application build.

### Strict Input Validation at the MCP Boundary

All incoming MCP requests are validated tightly before reaching service logic. The MCP layer is a trust boundary — external agents may send malformed or adversarial inputs, so validation is strict rather than permissive.

### Governance Tools Exposed via MCP

Governance actions (e.g., creating approvals) are available as MCP tools. This allows external agents to participate in Paperclip's approval workflow without direct API integration. The `approval-create` tool lets an MCP-connected agent request Board approval for a proposed action.
