---
title: "Public Roadmap & Contribution Policy"
owners: [bingran-you, cryppadotta, serenakeyitan, devinfoley]
---

Paperclip publishes a public roadmap (`ROADMAP.md` at the repo root) that serves as the authoritative list of in-scope initiatives for the project. The roadmap is the source of truth for what the core team is actively pursuing and what contributions will be accepted.

**Contribution policy tied to roadmap.** Feature PRs from external contributors must align with items on the published roadmap. Contributors are expected to check `ROADMAP.md` before starting non-trivial feature work, and the PR template (`.github/PULL_REQUEST_TEMPLATE.md`) prompts submitters to reference the relevant roadmap item. Out-of-scope feature PRs may be closed or deferred even if technically sound — the gate is strategic fit, not code quality.

**Why this exists.** As the project grows, unsolicited feature PRs create review burden and can pull the codebase in directions that conflict with the core team's plans. Publishing the roadmap makes scope decisions transparent and lets contributors self-select work that is likely to land. Bug fixes, adapter additions, and documentation improvements remain welcome outside the roadmap — the policy targets net-new features.

**How to apply.** When reviewing or advising on a feature PR: check whether it maps to a `ROADMAP.md` entry. If it doesn't, flag this to the contributor early rather than deep into review. When planning new tree nodes for features, the roadmap is a useful signal for what's coming and what deserves a node now vs. later.
