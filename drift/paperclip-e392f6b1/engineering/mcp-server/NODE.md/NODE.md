---
title: "MCP Server"
owners: []
---

# MCP Server

Standalone Model Context Protocol (MCP) server package that exposes Paperclip's capabilities to MCP-compatible AI clients. This package allows external agents and tools (e.g., Claude Desktop, IDE extensions) to interact with Paperclip companies, agents, tasks, and governance workflows through the standardized MCP protocol.

## Architectural Context

The MCP server is a separate package in the monorepo (`packages/mcp-server` or similar), distinct from the main Express backend. It implements the MCP specification to expose Paperclip resources and tools as MCP primitives — resources for reading state (companies, agents, tasks, issues) and tools for mutations (creating tasks, approving actions, managing agents).

## Key Decisions

- **Standalone package.** The MCP server is a separate entry point, not embedded in the main backend server. This allows it to be run independently or bundled for distribution to MCP hosts.
- **Reuses core service logic.** The MCP server delegates to the same service layer as the REST API, ensuring consistent business logic and governance enforcement.
- **Company-scoped access.** MCP sessions are scoped to a company, maintaining the platform's multi-tenant isolation model.

## Boundaries

This node covers the MCP server package architecture, tool/resource surface, and integration decisions. The underlying business logic lives in `../backend`. Adapter-specific MCP concerns (if any) live in `../../adapters`.
