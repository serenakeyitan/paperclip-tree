---
title: "MCP Server"
owners: [@bingran-you, @serenakeyitan, @bingran-you, @serenakeyitan]
---

# MCP Server

Model Context Protocol server exposing Paperclip operations as MCP tools for AI agents. Source: `/server/src/mcp/`.

## Purpose

Provides a standardized tool interface so that external AI agents (Claude, Codex, etc.) can interact with Paperclip entities — issues, approvals, agents, projects — without going through the HTTP API directly. The MCP server is included in the Docker image and its manifest is bundled in the deps stage.

## Available Tools

- **Approval creation** — Agents can programmatically create approval requests, enabling governance workflows to be triggered from within agent sessions rather than only through the UI or API.

## Key Decisions

### Validation

API request validation is strict — the MCP layer validates inputs before forwarding to services, preventing malformed requests from reaching business logic. This was tightened after early usage revealed agents sending loosely-typed payloads.
