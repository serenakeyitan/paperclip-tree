---
type: TREE_STALE
source_id: paperclip-e392f6b1
source_commit_range: 4d61dbfd34389a4723d6ae3df22df53501b42c71..11de5ae9c9523064a290755c02fb66e4e1c1b1e3
target_node: engineering/frontend/NODE.md
supersedes: engineering/frontend/NODE.md
rationale: The frontend node lists pages but doesn't mention keyboard shortcuts framework, assistant-ui integration, UX Lab, image gallery, or the shimmer animation system — all significant new UI patterns.
---
The existing node should be updated to add:

- **assistant-ui** as a key dependency for chat-style issue threads
- **Keyboard shortcuts framework** — Global keyboard shortcut system with a ? cheatsheet dialog, g-prefixed navigation shortcuts, and j/k traversal
- **Image gallery** — Clicking images in chat messages opens a gallery overlay; attachments render as square-cropped grids
- **Shimmer animations** — Working/Worked state indicators using oklch-based shimmer gradients with pause-between-repeats timing
- **UX Lab** — Experimental UI surface for testing interaction patterns before promoting to production
