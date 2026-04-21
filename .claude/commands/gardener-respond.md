Acknowledge reviewer feedback on a sync PR (a PR opened against this
tree repo by `first-tree tree sync`). Run from inside the tree repo.

This wraps `first-tree gardener respond`. Single-PR only — there is no
scan mode. Discovery for respond lives in the breeze notification
poller.

## Preflight

```bash
if [ ! -f ".claude/gardener-config.yaml" ]; then
  echo "❌ No .claude/gardener-config.yaml. Run /gardener-onboarding first."
  exit 1
fi

if ! gh auth status >/dev/null 2>&1; then
  echo "❌ gh CLI not authenticated. Run: gh auth login"
  exit 1
fi
```

## Invoke

```bash
npx -p first-tree first-tree gardener respond --pr <n> --repo <owner>/<tree-repo>
```

Current behavior (Phase 5): the CLI runs the edit orchestrator. For
the `parent_subdomain_missing` pattern it edits `NODE.md`, commits,
and pushes to the PR branch (`git push origin HEAD` only — no
force-push). For unsupported patterns it falls back to a placeholder
acknowledgement reply and bumps the attempts counter. See
[first-tree#160](https://github.com/agent-team-foundation/first-tree/issues/160)
and [first-tree#219](https://github.com/agent-team-foundation/first-tree/issues/219)
for scope.

## Dry run

```bash
npx -p first-tree first-tree gardener respond --pr <n> --repo <owner>/<tree-repo> --dry-run
```

Prints the planned edit plan, commit, and push without executing.

## When respond defers

The orchestrator returns one of:

- `applied` → real edit landed, real reply posted; attempts counter
  unchanged.
- `deferred` (e.g. `rebase_needed`, `stale_base`, `unsupported_pattern`)
  → placeholder reply, attempts counter bumped. `rebase_needed` means
  the sync branch can't fast-forward against main; resolve manually
  (use the GitHub "Update branch" button — never force-push).
- `failed` → the CLI exits non-zero with the error in its log; no
  reply posted, attempts counter unchanged.
