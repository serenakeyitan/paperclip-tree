---
title: "Issue References"
owners: [bingran-you, cryppadotta, serenakeyitan]
soft_links: ["product/task-system/issue-blockers/NODE.md"]
---

# Issue References

Issue references are first-class mentions of one issue inside another issue's title, description, comments, or documents. When text contains an identifier like `PAP-123`, a relative path like `/issues/PAP-123`, or a canonical URL, the system recognizes it as a structured link rather than plain text.

## Key Decisions

### Mentions Tracked in a Dedicated Table

Reference mentions are persisted in an `issue_reference_mentions` table that records the source issue, target issue, source kind (`title`, `description`, `comment`, `document`), and the originating record. This enables fast bidirectional lookup (outbound and inbound) without re-scanning content on every request, and company-scoped indexes keep queries isolated per company.

**Rationale:** Re-parsing issue bodies on every render does not scale, and denormalizing mentions into their own table is the cheapest way to get inbound lookup. Company-scoping the index follows Paperclip's general isolation rule — references never leak across companies.

### Automatic Sync on Write, Manual Backfill for History

Future issue, comment, and document writes sync references automatically — the schema migration does not backfill historical content. Operators run `pnpm issue-references:backfill` (optionally scoped with `--company <id>`) once after migrating to populate mentions for existing issues. This keeps the migration cheap and makes backfill an explicit operator choice.

**Rationale:** Backfilling inside the migration would make the migration unbounded in cost and risky for large tenants. Making backfill a separate operator step keeps the migration fast and lets operators pick a safe window per company.

### Related Work as a Derived Summary

Issues expose a `relatedWork` summary with `outbound` and `inbound` items, each carrying a mention count and the list of source contexts. This is distinct from blockers and issue relations: related-work is emergent from natural-language references, not an explicit relationship the user declares. Blockers and relations remain the mechanism for semantic links with behavioral side effects.

**Rationale:** Agents and humans often write "see PAP-123" without wanting the heavier semantics of a blocker. Surfacing these as a derived summary gives navigation value without overloading the relationship types that drive automated behavior (see [issue-blockers/](../issue-blockers/NODE.md)).

### Canonical Identifier and Href Format

Identifiers follow `[A-Z]+-\d+` and are normalized to uppercase. Canonical hrefs are `/issues/<IDENTIFIER>`, and the parser also accepts `/<PROJECT>/issues/<id>` paths and absolute URLs. Parsing strips markdown code blocks so references inside fenced code are not mistakenly linked.

**Rationale:** A single canonical form keeps indexing and dedup straightforward. Stripping code fences avoids false positives when agents paste logs or snippets that happen to contain identifier-shaped tokens.

## Relationship to Issue Blockers

Issue references and [issue blockers](../issue-blockers/NODE.md) are complementary, not competing. Blockers are explicit, user-declared relationships with behavioral side effects (dependency wakeups, agent coordination). References are emergent links derived from natural-language mentions. An issue can be both referenced and blocked by another issue; they travel in different tables and drive different UI surfaces.

## Source

- paperclipai/paperclip#4214 — Add first-class issue references
- Filed via `first-tree gardener sync --open-issues` (#277), proposal tracked in #326.
