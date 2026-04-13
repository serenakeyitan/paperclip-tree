---
title: "Document Diff Rendering"
owners: [bingran-you, cryppadotta, serenakeyitan]
---

# Document Diff Rendering

How document edits (issue description changes) are displayed inline within the issue thread, so agents and humans can see what changed without navigating to a separate diff view.

## Key Decisions

### Inline Diff Display

When an issue document is edited (by an agent updating the description or a human revising acceptance criteria), the diff is rendered inline in the issue thread rather than requiring navigation to a separate view. This keeps the full edit history visible in the natural reading flow of the thread.

### Rendering Inlined into Thread Components

Document diff rendering was refactored to be inlined directly into the thread rendering pipeline rather than being a separate component or view. This reduces architectural indirection and ensures diffs follow the same rendering path as other thread content (comments, status changes, etc.).
