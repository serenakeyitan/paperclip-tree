---
title: "MCP Server"
owners: [@bingran-you, @serenakeyitan, @bingran-you, @serenakeyitan]
---

# MCP Server

Model Context Protocol server exposing Paperclip capabilities as tools for external AI agents. Source: `/mcp`. Package: `@paperclipai/mcp`.

## Architecture

The MCP server is a separate entry point from the main backend, providing a tool-based interface that external agents (Claude, Codex, etc.) can call. It shares service-layer logic with the backend but enforces its own request validation at the boundary.

### Trust Boundary

MCP is an external-facing integration surface. All incoming requests go through strict schema validation before reaching service logic. This is a security-critical boundary — every tool input is validated, not just authenticated.

## Tools

### approval-create

Programmatic approval creation for agents that need to request human sign-off. This connects MCP to the governance approval workflow, allowing external agents to trigger approval gates without direct API access.

## Deployment

The MCP server manifest is included in the Docker deps stage to ensure the MCP package and its dependencies are available in production images. CI pipelines handle manifest changes independently of lockfile updates.
