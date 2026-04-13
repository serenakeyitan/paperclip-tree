---
title: "MCP Server"
owners: [@bingran-you, @serenakeyitan, @bingran-you, @serenakeyitan]
---

# MCP Server

Standalone Model Context Protocol server that exposes Paperclip capabilities to external AI agents (Claude Code, Cursor, etc.).

**Source:** `packages/mcp/`
**Package:** `@paperclipai/mcp`

## Architecture

The MCP server imports backend service functions directly rather than duplicating business logic — the backend remains the single source of truth for all operations. The server acts as a transport layer; governance gates and permissions still apply.

### Trust Boundary

MCP callers are treated as external/untrusted. Strict validation is applied at the MCP boundary, independent of (and stricter than) REST route validation. This is defense-in-depth — even if an MCP caller has valid credentials, inputs are re-validated.

### Tool Discovery

Tools are declared in a manifest file. Unlisted actions are unreachable. Tool additions require manifest updates. The manifest is included in the Docker `deps` stage for correct layer caching, and CI supports manifest-only changes without lockfile updates.

## Tools

- **`approval-create`** — Programmatically create Paperclip approval requests, enabling agents to trigger governance gates without direct REST API access.
