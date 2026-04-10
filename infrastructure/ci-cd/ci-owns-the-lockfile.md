---
title: "CI Owns The Lockfile"
owners: [bingran-you, cryppadotta, serenakeyitan]
---

# CI Owns The Lockfile

In a pnpm workspace with frequent package changes, hand-edited lockfiles create
merge churn and make it harder to tell whether dependency state is trustworthy.

## Decision

Treat `pnpm-lock.yaml` as CI-owned state. Regular pull requests are not allowed
to edit it directly; the dedicated lockfile refresh workflow is the canonical
path for lockfile updates.

## Why

This reduces noisy merge conflicts, keeps dependency refreshes explicit, and
helps ensure that install reproducibility is enforced by automation rather than
left to contributor discipline.

## Implications

- The PR policy workflow should keep rejecting manual lockfile edits except for
  the designated refresh path.
- Manifest changes may require or trigger an automated lockfile refresh PR.
- Dependency debugging should distinguish between source changes and lockfile
  state changes, since they are intentionally separated.
- Docker and CI install behavior remain aligned around a single lockfile source
  of truth.

## Related Domains

- [infrastructure deployment](../deployment/NODE.md)
- [infrastructure testing](../testing/NODE.md)
- [engineering](../../engineering/NODE.md)
