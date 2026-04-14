Stop all gardener automation for this project.

## 1. Stop comment module

Check if `.claude/commands/gardener-comment-stop.md` exists.
- If yes → read and execute it as a runbook.
- If no → skip.

## 2. Stop sync module

Check if `.claude/commands/gardener-sync-stop.md` exists.
- If yes → read and execute it as a runbook.
- If no → skip.

## 3. Confirm

Output:
"🌱 gardener stopped.
- Comment module: <stopped | not installed>
- Sync module: <stopped | not installed>
- Restart: `/gardener-start`"
