---
type: TREE_SUPPLEMENT
source_id: paperclip-e392f6b1
source_commit_range: db4e1465517f6e96876dda85488d4ab7210412a1..5d1ed71779df5622d9fd99ad28816b2da4bdee31
target_node: new
rationale: The existing node covers rendering and interaction patterns for issue threads but does not mention performance optimization for long comment threads, which this PR addresses.
---
### Performance: Long Comment Threads

Long comment threads (many replies) can cause typing lag in the input area. The thread renderer avoids re-rendering the full comment list on every keystroke by isolating the compose input from the thread content. This is a frontend performance concern specific to the thread view, not a general rendering issue.
