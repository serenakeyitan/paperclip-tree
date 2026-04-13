---
type: TREE_MISS
source_id: paperclip-e392f6b1
source_commit_range: 45ebecab5a3d404970f555d1750dc73cf2b3a2be..5d1ed71779df5622d9fd99ad28816b2da4bdee31
target_node: new
rationale: The PR introduces a document revision freshness pattern (keeping the latest issue document revision current) that no existing node covers — existing issue-thread-ux nodes cover markdown rendering and routing, not data currency for document revisions.
---
# Issue Document Freshness

How the frontend ensures the displayed issue document always reflects the latest revision, preventing stale content from being shown to agents and humans.

## Key Decisions

### Keep Latest Revision Current

When an issue's document content is updated (by an agent editing the description, or a human revising it), the UI ensures the displayed revision is always the most recent one. This prevents a common class of bugs where a user sees an outdated document revision while newer content exists on the server.

**Rationale:** Autonomous agents frequently update issue descriptions as they work — adding implementation notes, updating acceptance criteria, or appending status. A human operator viewing a stale revision could make governance decisions based on outdated information. Ensuring revision currency is a correctness concern, not just a UX polish.

### Frontend-Driven Revision Tracking

Revision currency is enforced in the frontend layer rather than relying solely on server push. The UI tracks the current document revision and reconciles it against incoming data to ensure the latest version is always displayed.

Source: PR `fix(ui): keep latest issue document revision current`
