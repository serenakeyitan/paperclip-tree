---
type: TREE_MISS
source_id: paperclip-e392f6b1
source_commit_range: a3e125f79659e9d6a2caac8ff3a0eb3cd4127039..d6b06788f6efacb002791c1a60b4889d7bfdb22d
target_node: new
rationale: Multiple commits add MCP server functionality (approval creation tool, manifest in Docker, API validation) but the tree has zero MCP coverage.
---
## Purpose

Paperclip exposes an MCP (Model Context Protocol) server that allows external AI agents and tools to interact with the platform programmatically. The MCP server provides tools for creating and managing Paperclip entities — approvals, issues, and other resources — through the standardized MCP protocol.

## Key Decisions

- **Approval creation tool** — External agents can create approval requests via MCP, enabling governance workflows to be triggered from any MCP-compatible client.
- **Manifest-based tool discovery** — The MCP server uses a manifest file that declares available tools. This manifest must be included in the Docker deps stage for production builds.
- **Strict API request validation** — All MCP tool inputs are validated tightly to prevent malformed requests from reaching backend services.

## Relationship to Other Domains

- **Governance** — The MCP approval tool is a programmatic interface to the approval gates described in `product/governance/NODE.md`.
- **Deployment** — The MCP server manifest is part of the Docker build pipeline (`infrastructure/deployment/NODE.md`).
- **Backend** — MCP tools call into the same backend services described in `engineering/backend/NODE.md`.
