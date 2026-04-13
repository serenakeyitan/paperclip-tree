---
title: "MCP Server"
owners: []
---

## Overview

Standalone Model Context Protocol (MCP) server that exposes Paperclip capabilities to MCP-compatible AI clients (Claude Desktop, IDE extensions, external agents). Runs as its own process, separate from the Express backend, but reuses core service logic.

## Key Decisions

- **Separate package, shared logic.** The MCP server is a distinct entry point (`packages/mcp-server`) that imports service modules from the backend rather than duplicating them. This keeps the MCP surface thin while the backend remains the source of truth for business logic.
- **Company-scoped access control.** Every MCP tool call is scoped to a `company_id`, consistent with the platform-wide isolation model.
- **Strict request validation.** API inputs are validated tightly at the MCP boundary before forwarding to backend services, since MCP clients are untrusted external callers.
- **Docker-aware packaging.** The MCP server manifest is included in the Docker deps stage so that container builds can resolve its dependencies without a full workspace install.

## Current Tools

- `approval-create` — Allows external agents to create approval requests through the MCP interface, integrating with the governance approval gates documented in `product/governance/`.

## Relationships

- Depends on backend service layer (`engineering/backend/`)
- Integrates with governance approval system (`product/governance/`)
- Packaged alongside other workspace packages (`infrastructure/deployment/`)
