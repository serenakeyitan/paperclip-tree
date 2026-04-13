---
type: TREE_MISS
source_id: paperclip-e392f6b1
source_commit_range: fceefe7f097543bd565309b2290f26ee19191493..b649bd454fce0c5d9aed64e6b75eb302b5d255ba
target_node: new
supersedes: null
rationale: Issue documents now support revision tracking with a diff viewer — a new document versioning capability (revision diff viewer, inline diff rendering, latest revision tracking) not mentioned in the task system node.
---
## Issue Document Revisions

Issue documents support revision tracking with visual diff comparison between versions.

### Features

- **Revision history** — Each edit to an issue document creates a new revision, maintaining a full history of changes.
- **Diff viewer** — A dedicated diff modal shows changes between revisions with inline rendering. The modal uses a 90% viewport width for readability.
- **Latest revision tracking** — The UI keeps the latest document revision current, ensuring agents and users always see the most recent version.

### Decisions

- Inline diff rendering rather than side-by-side — optimized for the typical issue document width.
- Revisions are stored server-side, not computed client-side from edit history.
