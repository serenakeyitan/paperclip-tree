---
title: "MCP Server"
owners: []
---

## MCP Server

Paperclip exposes an MCP (Model Context Protocol) server that lets external AI agents interact with the platform through standardized tool calls. The MCP server is bundled into the Docker image alongside the main backend.

### Key Decisions

- **Tool-based interface** — Each capability (e.g. creating approvals, querying issues) is exposed as a discrete MCP tool with validated request schemas.
- **Validation at the boundary** — All MCP API requests are tightly validated before reaching internal services. This is the trust boundary between external agents and Paperclip internals.
- **Docker packaging** — The MCP server manifest is included in the Docker deps stage so it is available at build time. Changes to the manifest must be reflected in the Dockerfile.

### Tools

- `approval-create` — Creates approval requests programmatically, enabling agents to trigger governance workflows via MCP.

### Cross-Domain Links

The MCP server bridges the governance model (approval gates, budget controls) with external agent runtimes. Adapter-connected agents can invoke MCP tools to participate in governance workflows without direct API access.
