---
title: "MCP Server"
owners: [cryppadotta]
---

# MCP Server

Standalone Model Context Protocol (MCP) server that exposes Paperclip capabilities as tools for external AI agents (Claude Desktop, IDE extensions, external agents). Source: `/packages/mcp-server`.

Package: `@paperclipai/mcp-server`

## Architecture

### Separate Package, Shared Services

The MCP server is a distinct package and entry point that imports backend service modules rather than duplicating business logic. This keeps the MCP protocol boundary clean while the backend remains the single source of truth for business rules.

### Trust Boundary

MCP callers are external agents — untrusted callers with different trust characteristics than authenticated UI users. All incoming MCP requests are strictly validated at the boundary before reaching internal services, independently of backend route validation.

### Company-Scoped Access

Every MCP tool call is scoped to a `company_id`, consistent with the platform-wide isolation model.

## Current Tools

- **`approval-create`** — Allows external agents to create approval requests programmatically, integrating with the governance approval gate system.

## Deployment

The MCP server manifest is included in the Docker deps stage so container builds can resolve its dependencies without a full workspace install. Changes to the manifest must be reflected in the Dockerfile.

## Cross-Domain

- Depends on backend service layer (`engineering/backend/`)
- Integrates with governance approval system (`product/governance/`)
- Bridges adapter-connected agents with governance workflows without requiring direct API access
