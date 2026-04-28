---
title: "Targeted Checks Before PR"
owners: [bingran-you, cryppadotta, serenakeyitan]
soft_links: [engineering/contributor-guide/NODE.md, infrastructure/testing/test-command-tiers/NODE.md]
---

Paperclip's contributor workflow distinguishes between *in-flight* verification and *PR-ready handoff* verification. For normal issue work, contributors and agents should start with the smallest targeted check that proves the change — a single test file, a focused typecheck on the touched package, or a narrow build — rather than running repo-wide commands on every iteration.

Repo-wide typecheck, build, and test runs are reserved for PR-ready handoff, or for changes broad enough that narrow checks do not cover the risk (cross-package refactors, shared-type changes, dependency bumps). The intent is to keep iteration fast and avoid burning budget on whole-monorepo runs that don't add signal for a localized change.

This policy applies equally to human contributors and to agent heartbeats running issues: an agent should not default to `pnpm test` or `pnpm build` at the repo root for every change. The judgment call — "is my change narrow enough that targeted checks suffice?" — is part of the work, and should be reflected in the comment trail when handing off.

See `doc/DEVELOPING.md` for the canonical phrasing.
