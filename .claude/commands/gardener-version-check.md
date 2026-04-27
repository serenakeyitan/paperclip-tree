<!-- Shared preflight. Called at the top of every user-facing gardener
     entry point. Enforces "installed == latest release" for first-time
     and restart cases, but softens to a warning when gardener is
     already actively running (don't interrupt active users). -->

Verify repo-gardener is up to date before doing anything else.

## 0. Skip when unattended

```bash
: "${UNATTENDED:=false}"
if [ "$UNATTENDED" = "true" ]; then
  # /loop and /schedule invocations: skip the version check entirely.
  # These run every few minutes; hitting GitHub each time is wasteful,
  # and we can't prompt the user anyway. Upgrade nudging happens on
  # interactive commands.
  return 0 2>/dev/null || exit 0
fi
```

## 1. Read installed version

```bash
if [ ! -f .claude/gardener-config.yaml ]; then
  echo "❌ repo-gardener is not installed in this directory."
  echo "Run /gardener-onboarding first."
  exit 1
fi

installed_version=$(grep -E '^installed_version:' .claude/gardener-config.yaml | sed 's/.*: *//' | tr -d '"' || true)
```

If `installed_version` is empty or `(unknown)` → STOP:

> ❌ **repo-gardener version unknown.** Run `/gardener-upgrade` to
> install the latest release and record the version, then re-run
> what you were doing.

## 2. Resolve latest release tag

```bash
latest_version=$(gh api /repos/agent-team-foundation/repo-gardener/releases/latest --jq .tag_name 2>/dev/null || true)
```

If `latest_version` is empty (network error, rate limit, GitHub down)
→ log and continue. Do NOT block on a failed network check.

```bash
echo "⚠️  Could not reach GitHub to check for updates — proceeding with installed $installed_version."
```

Skip the rest of this file and return to the caller.

## 3. Up to date?

```bash
if [ "$installed_version" = "$latest_version" ]; then
  echo "✓ repo-gardener $installed_version (up to date)"
  # return to caller — proceed with the requested command
fi
```

Return to the caller.

## 4. Out of date — check if gardener is already running

If versions differ, decide enforcement level by whether gardener is
already actively running. Active users don't get hard-blocked
mid-flight — they get a warning.

### 4a. Probe for an enabled cloud schedule

Use the RemoteTrigger tool with `action: "list"`. Scan the results
for any trigger whose name is `repo-gardener` OR `gardener-sync` AND
whose `enabled` field is `true`.

- If at least one match → set `gardener_running=true`.
- If none, or the probe errors → leave `gardener_running=false`.

Do NOT probe for local `/loop` state — it is per-session and not
reliably detectable from another session. The cloud-schedule probe
is authoritative enough; if the user has a local-only install with
no cloud schedule, we treat them as "not running" and enforce
strictly, which is the safer bias (their loop stops when their
session ends anyway, so upgrading has no disruption cost).

### 4b. Enforce

**If `gardener_running=false`** → **STOP** (hard block):

> 🛑 **repo-gardener is out of date. Upgrade required.**
>
> - Installed: `$installed_version`
> - Latest:    `$latest_version`
>
> Run `/gardener-upgrade` before continuing. After upgrade, restart
> Claude Code so the new slash commands load, then re-run the
> command you just tried.

Exit the caller. Do NOT proceed past Step 0.

**If `gardener_running=true`** → **WARN + continue** (soft):

> ⚠️ **repo-gardener is out of date — continuing because gardener is already running.**
>
> - Installed: `$installed_version`
> - Latest:    `$latest_version`
>
> Run `/gardener-upgrade` at a convenient stopping point (e.g. before
> your next `/gardener-start`). Upgrade will require a Claude Code
> restart, so it's best done when you're not mid-flight.

Return to the caller — proceed with the requested command.
