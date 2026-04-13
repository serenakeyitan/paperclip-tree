---
title: "MCP Server"
owners: []
---

## Overview

Paperclip includes a standalone MCP (Model Context Protocol) server package that exposes Paperclip functionality as MCP tools. This allows external AI agents and clients to interact with Paperclip through the MCP standard.

## Key Decisions

- **Standalone package** — The MCP server is a separate package within the monorepo, not embedded in the main server. This keeps deployment flexible and allows independent versioning.
- **Docker inclusion** — The MCP server manifest is included in the Docker deps stage, ensuring it's available in production containers.
- **Approval creation tool** — The MCP server exposes an approval creation tool, enabling external agents to trigger approval workflows.
- **Request validation** — API request validation is tightened to prevent malformed or malicious input from MCP clients.
- **Manual publish workflow** — MCP server releases follow a manual publish process documented separately from the main CI/CD pipeline.
