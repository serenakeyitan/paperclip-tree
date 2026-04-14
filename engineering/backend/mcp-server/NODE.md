---
title: "MCP Server"
owners: [plind-dm]
---

# MCP Server

Model Context Protocol server that exposes Paperclip functionality as MCP tools for external AI agents and clients.

## Key Decisions

### Tool-Based API Surface

Paperclip capabilities (issue management, approvals, etc.) are exposed as discrete MCP tools that external agents can invoke. Each tool maps to an existing backend service operation.

### Approval Creation Tool

External agents can create approval requests through MCP, enabling governance workflows to be initiated from outside the Paperclip UI or CLI.

### Strict Request Validation

MCP API requests are tightly validated — malformed or unexpected fields are rejected rather than silently ignored. This prevents external agents from accidentally mutating state through ill-formed tool calls.

### Manifest in Docker Build

The MCP server manifest is included in the Docker dependency stage so that containerized deployments expose the correct tool definitions without a separate build step.
