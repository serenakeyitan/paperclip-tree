---
title: "Public Roadmap & Feature PR Policy"
owners: [bingran-you, cryppadotta, serenakeyitan, devinfoley]
---

Paperclip now publishes a public `ROADMAP.md` at the repo root that lists the features the core team plans to build. The roadmap is the canonical source for upcoming work direction and is referenced from `README.md` and `CONTRIBUTING.md`.

Contribution policy for feature PRs has been tightened: unsolicited feature PRs are not accepted by default. A feature PR is only eligible for review when it either (a) implements an item already on `ROADMAP.md`, or (b) implements a GitHub issue that a maintainer has pre-approved for community contribution. Bug fixes, documentation improvements, and small quality-of-life changes remain open to direct PRs without pre-approval.

The PR template (`.github/PULL_REQUEST_TEMPLATE.md`) has been updated to require contributors to state which roadmap item or pre-approved issue their feature PR addresses. Reviewers should reject or ask to convert to an issue any feature PR that does not cite a roadmap entry or pre-approved issue.

Why this lives in the tree: it is a cross-domain contribution rule that affects how maintainers triage PRs and how agents should scope proposed changes. Agents drafting feature work for Paperclip should first check `ROADMAP.md` and open/confirm an issue before writing code, rather than sending speculative feature PRs.
