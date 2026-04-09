<!-- Context: this runs in Anthropic's cloud via /schedule -->

**UNATTENDED=true** — you are running under `/schedule` in the cloud.
There is no user to prompt. If the runbook requires interactive input,
exit with an error log. The user must run `/gardener-manual` locally
to resolve.

The schedule's source repo is the **config repo** — it holds the
gardener commands and `.claude/gardener-config.yaml`. The `target_repo`
to review is inside that config file and is accessed only via `gh api`;
it is never cloned.

You MUST read `.claude/commands/gardener-manual.md` in this repo and
execute it as a strict runbook. Follow every step sequentially:
1. Execute the step
2. Log what you did to stdout
3. Move to the next step

Do not skip steps. Do not summarize. Do not stop early unless a step
explicitly says to exit or stop. If a step fails, follow the step's
own failure instructions. If no failure instructions exist, log the
error and continue to the next step.
