---
title: "MCP Server"
owners: [plind-dm]
---

# MCP Server

Model Context Protocol server that exposes Paperclip capabilities as MCP tools for external AI agents and clients.

Source: `/mcp-server`

Package: `@paperclipai/mcp-server`

## Key Decisions

### Tool-per-Action Mapping

Each Paperclip action (create issue, list agents, create approval, etc.) is exposed as a discrete MCP tool. This keeps the tool interface granular and discoverable rather than requiring callers to construct complex payloads.

### Strict Request Validation

All incoming MCP requests are validated against tight schemas before dispatching to backend services. This prevents malformed or over-broad requests from reaching the core API layer.

### Approval Creation via MCP

External agents can create governance approvals through the MCP interface, enabling AI agents connected via MCP to participate in the approval workflow without direct API access.

## Open Questions

- What is the full set of MCP tools exposed?
- How does MCP auth integrate with the existing actor model?
