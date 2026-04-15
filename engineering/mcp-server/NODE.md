---
title: "MCP Server"
owners: [bingran-you, cryppadotta, serenakeyitan]
---

# MCP Server

Standalone Model Context Protocol (MCP) server that exposes Paperclip capabilities as tools for MCP-compatible AI clients (Claude Desktop, IDE extensions, external agents). Source: `packages/mcp-server`.

Package: `@paperclipai/mcp-server`

## Key Decisions

### Separate Package, Shared Logic

The MCP server is a distinct entry point that imports service modules from the backend rather than duplicating business logic. This keeps the MCP surface thin while the backend remains the single source of truth for domain operations.

### MCP as Trust Boundary

The MCP server is a trust boundary — all incoming tool calls are strictly validated before reaching backend services. Malformed or unexpected payloads are rejected early, since MCP clients may be untrusted or semi-trusted external agents.

### Company-Scoped Access Control

Every MCP tool call is scoped to a `company_id`, consistent with the platform-wide isolation model defined in `product/company-model/`.

### Docker Manifest Inclusion

The MCP server manifest is included in the Docker deps stage so that production containers can serve the tool manifest without a full workspace rebuild.

## Current Tools

PR #3001 introduced the initial tool surface. The MCP server exposes the following tools:

- **`paperclipMe`** — Returns identity and context information about the authenticated agent/user.
- **`paperclipListIssues`** — Lists issues for a company, with optional filtering by status, assignee, and project.
- **`paperclipCreateIssue`** — Creates a new issue within a company and optional project context.
- **`paperclipCreateApproval`** — Allows external agents to programmatically create approval requests, integrating with governance approval gates (`product/governance/`).
- Additional read/write tools for comments, status transitions, and agent context established in the same PR.

## Relationships

- Depends on backend service layer (`engineering/backend/`)
- Integrates with governance approval system (`product/governance/`)
- Packaged alongside other workspace packages (`infrastructure/deployment/`)
