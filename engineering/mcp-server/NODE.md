---
title: "MCP Server"
owners: [bingran-you, cryppadotta, serenakeyitan]
---

# MCP Server

Standalone Model Context Protocol (MCP) server that exposes Paperclip capabilities to MCP-compatible AI clients (Claude Desktop, IDE extensions, external agents). Runs as its own process, separate from the Express backend, but reuses core service logic.

**Source:** `packages/mcp-server/`

## Key Decisions

### Separate Package, Shared Logic

The MCP server is a distinct entry point (`packages/mcp-server`) that imports service modules from the backend rather than duplicating them. This keeps the MCP surface thin while the backend remains the source of truth for business logic.

### Strict Request Validation

API inputs are validated tightly at the MCP boundary before forwarding to backend services, since MCP clients are treated as untrusted external callers. This is a stricter validation posture than the internal REST API.

### Company-Scoped Access Control

Every MCP tool call is scoped to a `company_id`, consistent with the platform-wide isolation model.

### Docker-Aware Packaging

The MCP server manifest is included in the Docker deps stage so that container builds can resolve its dependencies without a full workspace install.

## Current Tools

- `approval-create` — Allows external agents to create approval requests through the MCP interface, integrating with the governance approval gates.

## Boundaries

- Depends on backend service layer (`engineering/backend/`)
- Integrates with governance approval system (`product/governance/`)
- Packaged alongside other workspace packages (`infrastructure/deployment/`)
