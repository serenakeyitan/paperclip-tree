---
title: "Thread Performance"
owners: [bingran-you, cryppadotta, serenakeyitan]
---

# Thread Performance

Performance optimizations for the issue thread view when handling long comment threads — threads with many comments can cause input lag and sluggish interactions if not handled carefully.

## Key Decisions

### Typing Lag Fix for Long Threads

Long comment threads (many comments in a single issue) caused noticeable typing lag in the comment input. This was fixed in PR #3163 (PAPA-63). The fix ensures that re-renders triggered by the thread's comment list do not propagate into the text input component during active typing.

**Rationale:** As threads grow (common in agent-heavy workflows where agents post frequent status updates), naive re-rendering of the full comment list on every keystroke creates input lag. The thread view must isolate input state from comment list rendering.

### Performance Awareness for Agent-Heavy Threads

Agent workflows generate significantly more comments per issue than human-only workflows. Any future thread UI changes must be tested against threads with 50+ comments to catch performance regressions early.
