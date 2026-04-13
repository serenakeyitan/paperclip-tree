---
title: "MCP Server"
owners: []
---

## MCP Server

Standalone Model Context Protocol (MCP) server package that exposes Paperclip capabilities as MCP tools to external AI agents. Sits alongside the Express backend but runs as a separate entry point with its own manifest.

The MCP server reuses core service logic from the backend without duplicating business rules. It acts as a **trust boundary**: all incoming MCP requests are strictly validated at the boundary before reaching internal services. Access is company-scoped, consistent with Paperclip's isolation model.

### Current Tools

- **approval-create** — Enables external agents to trigger governance approval workflows on issues. This bridges the governance model (approval gates) with adapter-connected agent runtimes, allowing agents to participate in approval flows without direct API access.

### Deployment

The MCP server manifest is included in the Docker deps stage so the server can be built and deployed alongside the main backend. The manifest declares available tools and their schemas.

### Design Decisions

- **Separate package, shared services.** The MCP server is not a route on the Express backend — it is a distinct package that imports backend services. This keeps the MCP protocol boundary clean while avoiding business logic duplication.
- **Strict boundary validation.** Request validation is tightened at the MCP layer independently of backend route validation, because MCP callers are external agents with different trust characteristics than authenticated UI users.
