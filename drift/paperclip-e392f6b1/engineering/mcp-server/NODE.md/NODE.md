---
title: "MCP Server"
owners: [@bingran-you, @serenakeyitan, @bingran-you, @serenakeyitan]
---

# MCP Server

Model Context Protocol server exposing Paperclip tools to external AI agents. Source: `/mcp`.

Package: `@paperclipai/mcp`

## Purpose

Allows external AI agents (Claude Code, Cursor, etc.) to interact with Paperclip programmatically through the MCP standard. The server exposes a curated subset of Paperclip operations as MCP tools, starting with governance workflows.

## Key Decisions

### Approval Creation Tool

The first MCP tool is `approval-create`, which lets external agents request board approval programmatically. This aligns with the governance-first principle — even programmatic access goes through the same approval gates as the UI.

### Manifest-Driven Tool Registration

Tools are declared in a manifest that is included in the Docker build's dependency stage. This ensures the MCP server's tool surface area is version-controlled and reproducible across environments.

### Strict Request Validation

All API requests through the MCP server are tightly validated. The server does not pass through arbitrary payloads — each tool defines its exact input schema and rejects anything outside it.
