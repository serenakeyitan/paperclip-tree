---
title: "Issue Document Revisions"
owners: []
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
