Manually review source-repo PRs and issues against this Context Tree
and post structured verdict comments. Run from inside the tree repo.

This wraps `first-tree gardener comment`. The CLI owns all the heavy
lifting — evidence gathering, verdict templating, state markers,
self-loop guard, idempotency. This runbook just dispatches.

## Preflight

```bash
# Must be run from a tree repo.
if [ ! -f ".claude/gardener-config.yaml" ]; then
  echo "❌ No .claude/gardener-config.yaml. Run /gardener-onboarding first."
  exit 1
fi

# Must be authenticated to GitHub.
if ! gh auth status >/dev/null 2>&1; then
  echo "❌ gh CLI not authenticated. Run: gh auth login"
  exit 1
fi
```

## Scan mode — review every open PR/issue on the bound source repo

```bash
npx -p first-tree first-tree gardener comment
```

Reads `.claude/gardener-config.yaml` to find the bound source repo,
then walks every **open** PR and issue on it. Posts a structured
verdict comment against the tree on each. Idempotent: items already
reviewed at their current SHA are skipped via the state marker.

Scan mode does **not** open tree issues on merged PRs — that's the
single-item merge→tree-issue branch below.

## Single-item mode — one PR or issue

```bash
npx -p first-tree first-tree gardener comment --pr 42 --repo owner/app-repo
npx -p first-tree first-tree gardener comment --issue 7 --repo owner/app-repo
```

Reviews exactly the one item. This is the form breeze-runner calls on
a notification. When invoked on a **merged** PR that has a prior
gardener marker and `TREE_REPO_TOKEN` is set, it also opens a tree-repo
issue so the tree catches up with the merged decision.

Add `--assign-owners` to auto-assign NODE owners on the tree issue
(push-mode workflows set this by default).

## Dry run

```bash
npx -p first-tree first-tree gardener comment --dry-run
```

Prints every planned `gh` call without executing it. Use this on a new
config or after editing `.claude/gardener-config.yaml`.

## When gardener skips a PR

Common reasons (all logged):

- Only gardener has reviewed → self-loop guard; manual re-review
  requires a human review on the PR.
- The target PR is itself a `first-tree:sync` PR → use `/gardener-respond`.
- `modules.comment.enabled: false` in config → explicit opt-out.
- `TREE_REPO_TOKEN` unset on the merge→issue branch → that branch
  silently skips; the review itself still runs.
