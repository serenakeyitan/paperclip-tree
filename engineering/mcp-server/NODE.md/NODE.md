---
title: "MCP Server"
owners: [@bingran-you, @cryppadotta, @serenakeyitan]
---

# MCP Server

Standalone Model Context Protocol server that exposes Paperclip capabilities as MCP tools to external AI agents. Runs as a separate entry point from the Express backend but reuses core service logic.

Package: `@paperclipai/mcp`

## Architecture

### Separate Package, Shared Logic

The MCP server is a distinct package that imports backend service functions rather than duplicating business logic. This keeps the MCP protocol boundary clean while the backend remains the single source of truth for business rules.

**Rationale:** A single source of truth for business logic prevents drift between the UI-facing API and the agent-facing MCP surface.

### Strict Request Validation

All incoming MCP requests are validated tightly at the boundary before reaching internal services. MCP callers are external agents with different trust characteristics than authenticated UI users, so validation is independent of and stricter than backend route validation.

**Rationale:** MCP is a trust boundary. External agents don't go through the same auth flow as the UI. Treating every inbound MCP request as untrusted-by-default keeps the attack surface auditable and the tool catalog explicit.

### Docker-Aware Packaging

The MCP server manifest is included in the Docker deps stage so that layer caching works correctly when only tool definitions change.

## Tool Catalog

- **approval-create** — Lets external agents programmatically request approval, bridging MCP to Paperclip's governance gates.

## Cross-Domain Links

- **Governance** (`product/governance/`) — MCP tools are read/write but always subject to governance gates (approvals, budget hard-stops).
- **Backend** (`engineering/backend/`) — MCP tools call into the same backend services.
- **Deployment** (`infrastructure/deployment/`) — MCP manifest is part of the Docker build pipeline.
