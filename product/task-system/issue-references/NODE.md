# Issue References

First-class support for cross-issue references embedded in free-text content. When an issue title, description, comment, or document mentions another issue by its canonical identifier (e.g., `PAP-123`) or by an issue href (`/issues/PAP-123`, `/PAP/issues/pap-123`), the mention is detected, persisted, and surfaced as a navigable relationship.

## Key Decisions

### Mentions as a Distinct Relationship from Explicit Links

Reference mentions are tracked separately from explicit issue relations (blockers, duplicates, related-to). Explicit relations are deliberate structural links; mentions are incidental cross-references that emerge from natural writing. Both are useful but carry different semantic weight — mentions do not drive behavioral side effects the way blockers do, but they power a "related work" view so agents and humans can see everywhere an issue has been discussed.

### Canonical Identifier Format

Issue identifiers follow the `PREFIX-NUMBER` shape (uppercase letters, dash, digits) and are normalized to uppercase on ingest. The shared `issue-references` module owns detection (`findIssueReferenceMatches`), normalization (`normalizeIssueIdentifier`), and canonical href construction (`buildIssueReferenceHref`), so frontend rendering, backend scanning, and the backfill script all agree on what counts as a reference.

### Four Source Kinds

Mentions are attributed to one of four source kinds: `title`, `description`, `comment`, or `document`. This lets the related-work summary distinguish "mentioned in a comment" from "referenced in the plan document" and lets callers deduplicate per source record.

### Sync on Write, Backfill on Demand

Future writes to issue titles, descriptions, comments, and documents sync reference mentions automatically. The schema migration intentionally does not backfill historical content — operators run `pnpm issue-references:backfill` (optionally scoped to a single company) after migrating. This keeps the migration cheap and reversible and puts backfill cost under operator control.

### Company-Scoped Isolation

The `issue_reference_mentions` table is keyed on `company_id` alongside source/target issue ids, and all indexes lead with `company_id`. References never cross company boundaries, consistent with the broader company-scoped isolation principle.

## Surfaces

- **Backend service** (`server/src/services/issue-references.ts`) scans written content, upserts mentions, and powers the related-work summary returned on `Issue.relatedWork`.
- **Frontend** renders references as `IssueReferencePill` chips inside markdown bodies, with an `IssueRelatedWorkPanel` showing inbound and outbound mentions grouped by source kind.
- **Shared types** expose `IssueReferenceSource`, `IssueRelatedWorkItem`, and `IssueRelatedWorkSummary` so UI and server agree on the wire shape.
