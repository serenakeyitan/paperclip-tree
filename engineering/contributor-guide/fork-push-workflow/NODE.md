---
title: "Fork-First Push Workflow"
owners: [bingran-you, cryppadotta, serenakeyitan, devinfoley]
soft_links: [engineering/contributor-guide/NODE.md]
---

# Fork-First Push Workflow

When contributing feature branches to `paperclipai/paperclip`, the `paperclip-dev` skill prefers pushing to a user-owned fork over creating branches directly on the upstream repo. This keeps the upstream branch list clean and matches standard open-source contribution flow.

## Detecting the Fork Remote

Before pushing or opening a PR, run `git remote -v` and treat any remote whose URL points at `github.com:<user>/paperclip` (or `github.com/<user>/paperclip.git`) — where `<user>` is not `paperclipai` — as the user's fork. Common remote names are `fork`, `<username>`, or `myfork`. The remote pointing at `paperclipai/paperclip` is the canonical upstream and may be named either `origin` or `upstream` depending on how the user set up the repo; detection must work under either convention.

## Pushing and Opening PRs

Feature branches are pushed with `git push -u <fork-remote> HEAD`. PRs are then opened against the upstream repo using `gh pr create --repo paperclipai/paperclip --head <fork-owner>:<branch-name> ...`. The explicit `--head <owner>:<branch>` form is the reliable fallback when `gh pr create` cannot auto-detect the head ref from a fork-tracking branch.

## Fallback and Sync Rules

If `git remote -v` shows only `paperclipai/paperclip` remotes, fall back to pushing to `origin` as before. The skill explicitly forbids creating a fork on the user's behalf — ask first. Fork-sync commands fetch from and push to whichever remote (`origin` or `upstream`) points at the canonical upstream, so the workflow stays correct under either naming convention.
