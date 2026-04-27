Start all gardener automation for this project.

## Step 0: Version check

Read and execute `.claude/commands/gardener-version-check.md` as a
runbook. If it exits, stop here — do not proceed to the modules.

---

This starts all three modules:
1. **Comment** — reviews source repo PRs against the context tree
2. **Sync** — detects drift and opens tree-update PRs
3. **Respond** — reads review feedback, fixes sync PRs, learns for next time

## 1. Start comment module

Read and execute `.claude/commands/gardener-comment-start.md` as a runbook.
If it fails, log the error and continue to step 2.

## 2. Start sync module

Check if `.claude/commands/gardener-sync-start.md` exists.
- If yes → read and execute it as a runbook.
- If no → log "⏭ Sync module not installed. Run /gardener-onboarding to add it."

## 3. Start respond module

Check if `.claude/commands/gardener-respond-start.md` exists.
- If yes → read and execute it as a runbook.
- If no → log "⏭ Respond module not installed. Run /gardener-onboarding to add it."

## 4. Confirm

Output:
"🌱 gardener is running.
- Comment module: <started | failed | not installed>
- Sync module: <started | failed | not installed>
- Respond module: <started | failed | not installed>
- Stop all: `/gardener-stop`"
