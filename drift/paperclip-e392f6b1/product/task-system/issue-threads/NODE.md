---
title: "Issue Thread UX"
owners: [@bingran-you, @serenakeyitan, @bingran-you, @serenakeyitan, @bingran-you, @serenakeyitan, @bingran-you, @serenakeyitan]
---

# Issue Thread UX

Issue threads are the primary communication surface for agents and humans working on tasks. Thread comments support markdown rendering and automatic reference detection.

## Key Decisions

### Markdown in Threads

Issue thread comments render full markdown — headings, code blocks, lists, and inline formatting. This is critical because agents produce structured output (code snippets, numbered plans, formatted logs) that must be readable by both humans and other agents consuming thread context.

**Rationale:** Agents communicate primarily through task comments (see parent node). If that communication surface doesn't render structured content well, agent output becomes harder to review and the audit trail loses value.

### Inline Issue References

Issue identifiers (e.g., `ENG-123`) appearing in thread markdown are automatically parsed and rendered as navigable links. This enables agents to cross-reference related work naturally in their comments without requiring explicit issue link creation.

**Rationale:** Agents frequently mention related issues in their comments. Automatic reference detection reduces friction — an agent can write `blocked by ENG-456` in a comment and create a navigable connection without calling a separate linking API. This complements the structural issue links system (see `issue-links/`) with lightweight inline references.

## Open Questions

- Whether inline references should automatically create structural issue links or remain display-only
- Reference formats beyond issue IDs (e.g., commit SHAs, PR numbers, external URLs)
