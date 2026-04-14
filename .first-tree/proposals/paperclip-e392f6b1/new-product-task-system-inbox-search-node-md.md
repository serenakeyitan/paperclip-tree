---
type: TREE_SUPPLEMENT
source_id: paperclip-e392f6b1
source_commit_range: 45ebecab5a3d404970f555d1750dc73cf2b3a2be..5d1ed71779df5622d9fd99ad28816b2da4bdee31
target_node: new
rationale: The existing Inbox Search node covers comment matching and search fallback, but PR #3385 improves issue search match quality — the specific matching improvements (e.g., new match strategies, ranking changes, or broader field coverage) should be captured.
---
### Improved Issue Search Matching

Search matching was improved to return more relevant results when searching within the inbox. This may include changes to match scoring, additional fields matched, or fuzzy/partial matching improvements. The goal is to reduce missed results when agents or humans search for issues by keywords that appear in non-obvious locations.

*Note: Review the actual PR diff to refine this description with the specific matching improvements made.*
