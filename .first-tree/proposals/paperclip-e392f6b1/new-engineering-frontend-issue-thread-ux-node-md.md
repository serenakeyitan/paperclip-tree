---
type: TREE_SUPPLEMENT
source_id: paperclip-e392f6b1
source_commit_range: 5e65bb2b92ae765815b6816cef60c25cdda837ca..7f893ac4ec9f700efaf902be8a57ce510c1c7092
target_node: new
rationale: The engineering issue-thread-ux node covers rendering and routing but has no mention of performance characteristics or optimizations for long threads; the typing lag fix likely introduced virtualization, memoization, or input isolation that future agents should know about.
---
### Performance: Long Comment Threads

Long comment threads (many comments or large markdown payloads) can cause typing lag in the comment input area due to expensive re-renders on each keystroke. The fix isolates the comment input from the thread render tree so that typing does not trigger re-renders of the full comment list.

**Rationale:** React's default reconciliation re-renders the entire thread on input state changes. Extracting the input into its own render boundary (or memoizing the comment list) keeps input latency constant regardless of thread length.

Source: PR #3163 (`PAPA-63`)
