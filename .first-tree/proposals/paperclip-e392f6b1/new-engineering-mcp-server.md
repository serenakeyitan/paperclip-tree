---
type: TREE_MISS
source_id: paperclip-e392f6b1
source_commit_range: a3e125f79659e9d6a2caac8ff3a0eb3cd4127039..d6b06788f6efacb002791c1a60b4889d7bfdb22d
target_node: new
rationale: The PR adds an MCP approval-create tool and tightens MCP API validation, but the tree has no node for the MCP server package — only unapproved proposals exist.
---
## Overview

Standalone Model Context Protocol (MCP) server that exposes Paperclip capabilities to MCP-compatible AI clients (Claude Desktop, IDE extensions, external agents). Runs as its own process, separate from the Express backend, but reuses core service logic.

## Key Decisions

- **Separate package, shared logic.** The MCP server is a distinct entry point (`packages/mcp-server`) that imports service modules from the backend rather than duplicating them. This keeps the MCP surface thin while the backend remains the source of truth for business logic.
- **Company-scoped access control.** Every MCP tool call is scoped to a `company_id`, consistent with the platform-wide isolation model.
- **Strict request validation.** API inputs are validated tightly at the MCP boundary before forwarding to backend services, since MCP clients are untrusted external callers.
- **Docker-aware packaging.** The MCP server manifest is included in the Docker deps stage so that container builds can resolve its dependencies without a full workspace install.

## Current Tools

- `approval-create` — Allows external agents to create approval requests through the MCP interface, integrating with the governance approval gates documented in `product/governance/`.

## Relationships

- Depends on backend service layer (`engineering/backend/`)
- Integrates with governance approval system (`product/governance/`)
- Packaged alongside other workspace packages (`infrastructure/deployment/`)
