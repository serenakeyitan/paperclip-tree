---
title: "MCP Server"
owners: []
---

---
title: "MCP Server"
owners: [bingran-you, cryppadotta, serenakeyitan]
soft_links: ["engineering/backend/NODE.md", "product/governance/NODE.md", "infrastructure/deployment/NODE.md"]
---

# MCP Server

Standalone Model Context Protocol (MCP) server package that exposes Paperclip capabilities to MCP-compatible AI clients. External agents and tools (e.g., Claude Desktop, IDE extensions) interact with companies, agents, tasks, and governance workflows through the standardized MCP protocol.

**Source:** `packages/mcp-server/`
**Package:** `@paperclipai/mcp-server`

---

## Key Decisions

### Standalone Package, Shared Service Layer

The MCP server is a separate entry point from the main Express backend. It reuses the same core service logic, ensuring consistent business logic and governance enforcement. This allows independent distribution to MCP hosts while maintaining a single source of truth for business rules.

### Tool-Based Interface with Strict Validation

Each capability (e.g., approval creation, issue management) is exposed as a discrete MCP tool with a typed manifest. All incoming MCP API requests are validated tightly at the MCP layer before reaching backend services, preventing malformed or unauthorized tool invocations.

### Company-Scoped Access

MCP sessions are scoped to a company, maintaining the platform's multi-tenant isolation model. No cross-company data leakage through MCP tools.

### Governance Integration

MCP tools that create approvals or mutate governed resources go through the same approval gates, budget controls, and audit pipeline as the REST API and UI.

---

## Boundaries

- Underlying business logic lives in `../backend`.
- Governance rules (approval gates, budget stops) are documented in `../../product/governance`.
- The MCP server manifest is bundled in the Docker image (see `../../infrastructure/deployment`).
- Adapter-specific MCP concerns (if any) live in `../../adapters`.
