---
title: "MCP Server"
owners: [bingran-you, cryppadotta, serenakeyitan]
---

# MCP Server

Model Context Protocol server exposing Paperclip operations as MCP tools for external AI agents. Source: `/mcp-server`.

Package: `@paperclipai/mcp-server`

## Purpose

Provides a standardized interface for external AI agents (Claude, Codex, etc.) to interact with Paperclip programmatically via the Model Context Protocol. This is a separate entry point from the REST API, designed for agent-to-agent integration rather than human-facing UI.

## Key Decisions

### Thin Boundary Layer Over Backend Services

The MCP server imports and reuses backend service logic directly — it does not duplicate business logic. Routes in the MCP server are thin wrappers that translate MCP tool calls into backend service invocations. This keeps governance enforcement and audit trails consistent regardless of whether an action originates from the UI, REST API, or MCP.

### Strict API Request Validation

All MCP tool inputs are validated at the boundary before reaching backend services. External callers are untrusted by default. This is defense-in-depth — backend services also validate, but the MCP layer rejects malformed requests early.

### Available Tools

- **approval-create** — Create approval requests programmatically, enabling external agents to participate in governance workflows (e.g., requesting hiring approval or strategy sign-off via MCP rather than the UI).

### Docker Integration

The MCP server manifest is included in the Docker deps stage to ensure the MCP server is available in production container images.

## Relationship to Governance

MCP tools that create approvals or trigger governance-gated actions go through the same server-enforced approval workflow as UI and REST API actions. The MCP layer does not bypass governance checks.
