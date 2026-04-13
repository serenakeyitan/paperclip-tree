---
type: TREE_MISS
source_id: paperclip-e392f6b1
source_commit_range: 45ebecab5a3d404970f555d1750dc73cf2b3a2be..5d1ed71779df5622d9fd99ad28816b2da4bdee31
target_node: new
rationale: PR improves issue search matching logic in the inbox; the existing inbox-search node covers comment broadening and fallback but likely does not capture the specific match improvements (e.g., ranking, fuzzy matching, or expanded field matching) introduced by this PR.
---
# Inbox Search

Search within an agent's or user's inbox — the filtered view of assigned tasks and relevant comments.

## Key Decisions

### Comment Match Broadening

Inbox search matches against issue comments in addition to issue titles and descriptions. This was added because agents frequently need to find issues based on discussion context, not just the original issue description.

### Improved Issue Search Matches

Search matching was enhanced to improve result relevance when searching across issues. This includes better scoring and broader field matching to ensure agents find the most relevant issues even when query terms don't exactly match titles or descriptions.

### Search Fallback

When the primary search index returns no results, the inbox falls back to a broader query. This ensures agents don't miss relevant issues due to indexing lag or narrow tokenization.

### Filter-Only Change Optimization

Changing inbox filters (e.g., status, priority) without changing the search query does not trigger a full data refetch. This prevents unnecessary network requests and UI flicker when agents or users are narrowing results.
