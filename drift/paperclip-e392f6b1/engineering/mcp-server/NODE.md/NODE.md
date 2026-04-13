---
title: "MCP Server"
owners: []
---

Standalone Model Context Protocol (MCP) server package that exposes Paperclip's capabilities to MCP-compatible AI clients. This allows external agents and tools (e.g., Claude Desktop, IDE extensions) to interact with Paperclip companies, agents, tasks, and governance workflows through the standardized MCP protocol.

## Key Decisions

### Standalone Package

The MCP server is a separate entry point in the monorepo, not embedded in the main Express backend. This allows it to be run independently or bundled for distribution to MCP hosts. The server manifest is included in the Docker deps stage so it is available at runtime.

### Reuses Core Service Logic

The MCP server delegates to the same service layer as the REST API, ensuring consistent business logic and governance enforcement. The approval creation tool, for example, goes through the same validation and audit logging as the REST endpoint.

### Strict Request Validation

All incoming MCP API requests are validated tightly to prevent malformed or unauthorized tool invocations. Validation happens at the MCP layer before reaching backend services.

### Company-Scoped Access

MCP sessions are scoped to a company, maintaining the platform's multi-tenant isolation model.

## Current Tools

- **Approval creation** — external agents can create approval requests programmatically via MCP

## Boundaries

This node covers the MCP server package architecture, tool/resource surface, and integration decisions. The underlying business logic lives in [backend](../backend/NODE.md). Adapter-specific MCP concerns (if any) live in [adapters](../../adapters/NODE.md).
