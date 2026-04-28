---
title: "Issue Request Depth Clamp"
owners: [bingran-you, cryppadotta, serenakeyitan]
soft_links: [engineering/shared/NODE.md, product/task-system/NODE.md, product/task-system/issue-references/NODE.md]
---

# Issue Request Depth Clamp

Issues can chain into other issues through requests, forming a depth-bearing relationship. Paperclip caps that depth via the shared constant `MAX_ISSUE_REQUEST_DEPTH = 1024` and the helper `clampIssueRequestDepth`.

## Key Decisions

### Hard Ceiling at 1024

1024 is high enough that legitimate nested workflows never hit it, but low enough to prevent runaway chains — whether from buggy automation, recursive routines, or malicious input — from blowing up traversal, rendering, or storage. The cap is enforced at the shared layer so server, CLI, and any other consumer agree on the same limit without each re-deriving it.

### Defensive Clamping Semantics

`clampIssueRequestDepth` accepts `number | null | undefined`, returns `0` for non-finite or missing values, floors fractional inputs, and clamps the result into `[0, MAX_ISSUE_REQUEST_DEPTH]`. Callers can therefore feed it raw input (DB values, route params, imported data) without pre-validating, and the result is always a safe non-negative integer within the allowed range.
