<!-- Context: runs under /schedule or /loop for automated tree maintenance -->

**UNATTENDED=true** -- do not prompt the user.

## What this does

Runs three maintenance steps in sequence:
1. `first-tree sync` -- detect source drift and open tree-update PRs
2. Review open sync PRs -- deterministic + AI checks on `first-tree:sync` PRs
3. `gardener` review -- review open PRs on the tree repo for context fit

## Step 1: Run sync

Read and execute the **"Run"** section of `.claude/commands/gardener-sync-manual.md`
(the `first-tree sync --apply` command). This creates or updates tree PRs.
If it exits with an error, log the error and continue to Step 2.

## Step 2: Review sync PRs

Read and execute the **"Review sync PRs"** section of
`.claude/commands/gardener-sync-manual.md` (Steps 4a–4i). This scans all
open PRs labeled `first-tree:sync`, runs deterministic and AI checks,
handles any `@gardener re-sync` commands, and posts review comments.
If it exits with an error, log the error and continue to Step 3.

## Step 3: Run gardener

Check if `.claude/commands/gardener-comment-manual.md` exists.
- If yes -> read and execute it as a runbook.
- If no -> log "⏭ gardener not installed. Skipping context review."

## Summary

Log a one-line summary: "Sync: <N> PRs opened. Review: <N> sync PRs reviewed. Gardener: <N> items reviewed."
