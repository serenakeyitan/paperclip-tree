---
title: "MCP Server"
owners: []
---

## MCP Server

Paperclip exposes an **MCP (Model Context Protocol) server** that allows external AI agents and tools to interact with the platform programmatically. The MCP server provides a tool-based interface following the MCP specification.

### Available Tools

- **Approval creation** — External agents can create approval requests through the MCP interface, integrating with the governance approval gates.
- Additional tools will be added as the MCP surface expands.

### Design Decisions

- **Strict request validation** — All MCP API requests are tightly validated to prevent malformed or unauthorized tool invocations.
- **Packaged in Docker deps stage** — The MCP server manifest is included in the Docker dependency stage to ensure it is available at runtime in containerized deployments.
- **Governance integration** — MCP tools that trigger mutations (e.g., approval creation) go through the same governance and audit paths as UI-initiated actions.
