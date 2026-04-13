---
type: TREE_MISS
source_id: paperclip-e392f6b1
source_commit_range: a3e125f79659e9d6a2caac8ff3a0eb3cd4127039..d6b06788f6efacb002791c1a60b4889d7bfdb22d
target_node: new
rationale: The governance node documents only V1 hiring and CEO strategy approval gates, but this PR implements issue-level approvals — a third approval type accessible via MCP and the UI with dedicated board approval card styling.
---
Issue-level approval gates allow specific issues to require board or designated-authority approval before work proceeds. This extends the existing governance approval infrastructure (previously limited to hiring and CEO strategy decisions) to individual work items.

### Same Approval Infrastructure

Issue approvals reuse the existing approval system (board powers, approval cards, audit trail) rather than introducing a separate mechanism. This keeps governance consistent across all approval types.

### MCP-Accessible

External agents can create approval requests programmatically via the MCP `approval-create` tool, enabling automated governance workflows without direct API access.

### Relationship to Existing Gates

Issue approvals complement but do not replace the V1 hiring and CEO strategy gates. They operate at a finer granularity — per-issue rather than per-category — and can be triggered by agents or humans.
