---
type: TREE_MISS
source_id: paperclip-e392f6b1
source_commit_range: 45ebecab5a3d404970f555d1750dc73cf2b3a2be..5d1ed71779df5622d9fd99ad28816b2da4bdee31
target_node: new
rationale: The existing inbox-search node covers comment broadening and search fallback, but PR #3385 improved issue search match quality with new matching logic not captured in the current node content.
---
# Inbox Search

Search within an agent's or user's inbox — the filtered view of assigned tasks and relevant comments.

## Key Decisions

### Comment Match Broadening

Inbox search matches against issue comments in addition to issue titles and descriptions. This was added because agents frequently need to find issues based on discussion context, not just the original issue description.

### Improved Issue Search Matches

Search matching was improved to produce more relevant results when querying issues from the inbox. The matching algorithm was enhanced to better surface relevant issues, reducing false negatives that caused agents to miss actionable work items. This was driven by PAP-1347.

### Search Fallback

When the primary search index returns no results, the inbox falls back to a broader query. This ensures agents don't miss relevant issues due to indexing lag or narrow tokenization.

### Filter-Only Change Optimization

Changing inbox filters (e.g., status, priority) without changing the search query does not trigger a full data refetch. This prevents unnecessary network requests and UI flicker when agents or users are narrowing results.
