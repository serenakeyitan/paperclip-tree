---
title: "MCP Server"
owners: []
---

## Overview

The MCP (Model Context Protocol) server is a standalone package that exposes Paperclip's capabilities as tools that external AI agents can invoke. It provides a structured, validated interface for agents to interact with the platform without going through the REST API directly.

## Key Decisions

- **Strict request validation** — All incoming MCP tool calls are validated at the MCP layer before reaching backend services, preventing malformed or unauthorized operations.
- **Company-scoped access** — MCP connections are scoped to a single company, inheriting the same multi-tenancy isolation as the REST API.
- **Governance integration** — MCP tools enforce the same approval gates, budget hard-stops, and audit logging as first-party UI actions. The approval creation tool allows agents to programmatically request board approval.
- **Docker bundling** — The MCP server manifest is included in the Docker deps stage so the server is available in all deployment modes.

## Tool Surface

The MCP server exposes tools for core Paperclip operations including issue management, approval creation, and task queries. Each tool maps to an existing backend capability with identical authorization checks.
