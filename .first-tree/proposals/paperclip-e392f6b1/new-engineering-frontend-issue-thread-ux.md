---
type: TREE_MISS
source_id: paperclip-e392f6b1
source_commit_range: a3e125f79659e9d6a2caac8ff3a0eb3cd4127039..d6b06788f6efacb002791c1a60b4889d7bfdb22d
target_node: new
rationale: The tree documents issues exist as a page with 'detail view, chat UX, and inbox' but has no node capturing the issue thread UX pattern — markdown rendering in comment threads, issue cross-reference linking, and the threaded conversation model that is central to agent-to-agent and human-to-agent communication.
---
# Issue Thread UX

The issue thread is the primary communication surface in Paperclip. Since all agent communication flows through the task system (see [task-system](../../product/task-system/NODE.md)), the thread view is where humans and agents read and contribute updates, plans, and decisions.

## Markdown Rendering

Issue thread comments render full markdown with support for code blocks, tables, and inline formatting. The rendering pipeline handles both human-authored and agent-generated markdown, which often includes structured output like plans, diffs, and status reports.

## Issue References

Cross-references to other issues (e.g., `ENG-123`) are parsed and rendered as navigable links within thread comments. This enables traceability across the task hierarchy — agents and humans can reference parent issues, blocking issues, or related work inline.

## Key Decisions

- Thread markdown rendering must gracefully handle agent-generated content which may be verbose or contain unexpected formatting.
- Issue reference parsing follows the `{TEAM_KEY}-{NUMBER}` identifier format established in the task system.
- The thread UX prioritizes readability of long agent conversations over compact display.
