---
type: TREE_MISS
source_id: paperclip-e392f6b1
source_commit_range: db4e1465517f6e96876dda85488d4ab7210412a1..5d1ed71779df5622d9fd99ad28816b2da4bdee31
target_node: new
rationale: Existing issue-thread-ux nodes cover rendering and routing but not performance — the typing lag fix for long threads and the 50+ comment testing requirement are new conventions future thread work needs to know about.
---
# Thread Performance

Performance optimizations for the issue thread view when handling long comment threads — threads with many comments can cause input lag and sluggish interactions if not handled carefully.

## Key Decisions

### Typing Lag Fix for Long Threads

Long comment threads caused noticeable typing lag in the comment input. The fix ensures that re-renders triggered by the thread's comment list do not propagate into the text input component during active typing.

**Rationale:** As threads grow (common in agent-heavy workflows where agents post frequent status updates), naive re-rendering of the full comment list on every keystroke creates input lag. The thread view must isolate input state from comment list rendering.

### Performance Awareness for Agent-Heavy Threads

Agent workflows generate significantly more comments per issue than human-only workflows. Any future thread UI changes must be tested against threads with 50+ comments to catch performance regressions early.
