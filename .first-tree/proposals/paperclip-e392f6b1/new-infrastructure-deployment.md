---
type: TREE_SUPPLEMENT
source_id: paperclip-e392f6b1
source_commit_range: 5e65bb2b92ae765815b6816cef60c25cdda837ca..7f893ac4ec9f700efaf902be8a57ce510c1c7092
target_node: new
rationale: The PR adds the MCP server manifest to the Docker deps stage — a Docker build decision not captured in the deployment node's multi-stage Dockerfile documentation.
---
### MCP Server Manifest in Deps Stage

The MCP server manifest file is copied into the Docker deps stage alongside workspace `package.json` files. This ensures the MCP tool surface is available in production containers and that manifest-only changes don't unnecessarily bust the full source copy layer.
