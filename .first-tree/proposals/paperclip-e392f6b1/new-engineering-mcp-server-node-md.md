---
type: TREE_MISS
source_id: paperclip-e392f6b1
source_commit_range: fceefe7f097543bd565309b2290f26ee19191493..b649bd454fce0c5d9aed64e6b75eb302b5d255ba
target_node: new
supersedes: null
rationale: A standalone MCP server package was added to the monorepo with its own manifest, Docker inclusion, and approval creation tool — this is a new engineering domain not in the tree.
---
## MCP Server

A standalone Model Context Protocol (MCP) server package that exposes Paperclip operations as MCP tools. This allows external AI agents and IDEs to interact with Paperclip programmatically.

### Key Decisions

- **Separate package.** The MCP server is its own package in the monorepo (`mcp-server`), not embedded in the main backend. This keeps the MCP surface area isolated and independently publishable.
- **Tool surface.** Includes tools for approval creation and other issue operations. API request validation is tightened to prevent misuse.
- **Docker inclusion.** The MCP server manifest is included in the Docker deps stage so the server is available in production containers.

### Soft Links

- [Backend Server](../backend/NODE.md) — the MCP server delegates to the same backend services.
- [Adapters](../../adapters/NODE.md) — MCP is an alternative integration surface alongside adapters.
