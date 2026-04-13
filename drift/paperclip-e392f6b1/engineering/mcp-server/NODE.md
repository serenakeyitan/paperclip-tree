---
title: "MCP Server"
owners: [@bingran-you, @serenakeyitan, @bingran-you, @serenakeyitan]
---

# MCP Server

Model Context Protocol (MCP) server that exposes Paperclip capabilities as tools for external AI agents and IDEs. Source: `/mcp`.

## Purpose

Provide a standardized interface (following the MCP specification) so that AI coding assistants, IDE extensions, and other MCP-compatible clients can interact with Paperclip — creating issues, managing approvals, querying project state — without going through the REST API or UI.

## Key Decisions

### Tool-Based Interface

Each Paperclip capability exposed via MCP is modeled as a discrete tool with typed parameters and strict API request validation. Tools include approval creation, issue management, and project queries. Validation is tightened at the MCP boundary to reject malformed requests before they reach backend services.

### Packaged as Separate Manifest

The MCP server has its own manifest included in the Docker build's dependency stage, allowing it to be deployed alongside the main server without bloating the core image when MCP is not needed.

### Reuses Backend Services

MCP tool handlers delegate to the same service layer used by the REST API and CLI. No duplicated business logic — the MCP server is a thin protocol adapter over existing services.
