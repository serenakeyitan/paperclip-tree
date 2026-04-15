---
title: "MCP Server"
owners: [bingran-you, cryppadotta, serenakeyitan]
---

# MCP Server

Standalone Model Context Protocol (MCP) server that exposes Paperclip capabilities as tools for MCP-compatible AI clients (Claude Desktop, IDE extensions, external agents). Source: `packages/mcp-server`.

Package: `@paperclipai/mcp-server`

## Key Decisions

### Separate Package, Shared Logic

The MCP server is a distinct entry point that imports service modules from the backend rather than duplicating business logic. This keeps the MCP surface thin while the backend remains the single source of truth for domain operations.

### MCP as Trust Boundary

The MCP server is a trust boundary — all incoming tool calls are strictly validated before reaching backend services. Malformed or unexpected payloads are rejected early, since MCP clients may be untrusted or semi-trusted external agents.

### Company-Scoped Access Control

Every MCP tool call is scoped to a `company_id`, consistent with the platform-wide isolation model defined in `product/company-model/`.

### Docker Manifest Inclusion

The MCP server manifest is included in the Docker deps stage so that production containers can serve the tool manifest without a full workspace rebuild.

## Initial Tool Surface (PR #3001)

The MCP server ships a broad initial surface covering the core Paperclip domain objects:

**Read tools:** `paperclipMe`, `paperclipInboxLite`, `paperclipListAgents`, `paperclipGetAgent`, `paperclipListIssues`, `paperclipGetIssue`, `paperclipGetHeartbeatContext`, `paperclipListComments`, `paperclipGetComment`, `paperclipListIssueApprovals`, `paperclipListDocuments`, `paperclipGetDocument`, `paperclipListDocumentRevisions`, `paperclipListProjects`, `paperclipGetProject`, `paperclipListGoals`, `paperclipGetGoal`, `paperclipListApprovals`, `paperclipGetApproval`, `paperclipGetApprovalIssues`, `paperclipListApprovalComments`.

**Write tools:** `paperclipCreateIssue`, `paperclipUpdateIssue`, `paperclipCheckoutIssue`, `paperclipReleaseIssue`, `paperclipAddComment`, `paperclipUpsertIssueDocument`, `paperclipRestoreIssueDocumentRevision`, `paperclipCreateApproval`, `paperclipLinkIssueApproval`, `paperclipUnlinkIssueApproval`, `paperclipApprovalDecision`, `paperclipAddApprovalComment`.

**Escape hatch:** `paperclipApiRequest` — limited to paths under `/api` with JSON bodies, for endpoints without a dedicated MCP tool yet.

## Relationships

- Depends on backend service layer (`engineering/backend/`)
- Integrates with governance approval system (`product/governance/`)
- Packaged alongside other workspace packages (`infrastructure/deployment/`)
