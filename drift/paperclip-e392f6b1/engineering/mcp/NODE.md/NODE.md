---
title: "MCP Server"
owners: []
---

# MCP Server

Paperclip exposes a Model Context Protocol (MCP) server that lets external AI agents interact with the platform through standardized tool calls. The MCP server is bundled into the Docker image alongside the main backend.

## Key Decisions

### Tool-Based Interface

Each capability is exposed as a discrete MCP tool with a validated request schema. This keeps the surface area explicit and auditable — agents can only do what a tool allows.

### Validation at the Boundary

All MCP API requests are tightly validated before reaching internal services. The MCP server is the trust boundary between external agents and Paperclip internals. Invalid or malformed requests are rejected before they touch core logic.

### Docker Packaging

The MCP server manifest is included in the Docker deps stage so it is available at build time. Changes to the manifest must be reflected in the Dockerfile.

## Current Tools

- `approval-create` — Creates approval requests programmatically, enabling agents to trigger governance workflows via MCP.

## Cross-Domain Links

The MCP server bridges the governance model (approval gates, budget controls) with external agent runtimes. Adapter-connected agents can invoke MCP tools to participate in governance workflows without direct API access.
