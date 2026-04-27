<!-- Context: this runs locally via /loop on the user's machine -->

**UNATTENDED=true** — you are running under `/loop`. Do not prompt the
user. If the runbook requires interactive input (e.g. missing config,
ambiguous context tree URL), exit with an error log and wait for the
user to run `/gardener-comment-manual` interactively.

**RUN_MODE=loop** — set this in the environment so Step 5b records the
correct mode in `~/.gardener/runs.jsonl`.

You MUST read `.claude/commands/gardener-comment-manual.md` in this repo and
execute it as a strict runbook. Follow every step sequentially. For
each step:
1. Execute the step
2. Log what you did to stdout
3. Move to the next step

Do not skip steps. Do not summarize. Do not stop early unless a step
explicitly says to exit or stop. If a step fails, follow the step's
own failure instructions (which may include stopping). If no failure
instructions exist, log the error and continue to the next step.
