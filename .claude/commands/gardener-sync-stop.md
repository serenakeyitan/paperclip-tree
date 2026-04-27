Stop all gardener sync automation for this tree repo.

## 1. Stop local loop

Check if a `/loop` is currently running `/gardener-sync-loop`.
If yes, use `/loop stop` to stop it. If `/loop stop` is not available,
tell the user: "Please stop the local loop manually by pressing Ctrl+C
in the session running `/loop`."

## 2. Stop any open watch window

`/gardener-sync-watch` records its PID at `~/.gardener/sync-watch.pid`.
Kill it so the tail stops.

```bash
PIDFILE="$HOME/.gardener/sync-watch.pid"
watch_stopped="no watch window running"
if [ -f "$PIDFILE" ]; then
  watch_pid=$(cat "$PIDFILE" 2>/dev/null)
  if [ -n "$watch_pid" ] && kill -0 "$watch_pid" 2>/dev/null; then
    kill "$watch_pid" 2>/dev/null && watch_stopped="stopped (PID $watch_pid)"
  fi
  rm -f "$PIDFILE"
fi
echo "$watch_stopped"
```

## 3. Disable cloud schedule

List all scheduled triggers with the RemoteTrigger tool (`action: "list"`).
Find any trigger whose name is `gardener-sync`.
For each one, disable it with the RemoteTrigger tool:
`action: "update"`, `trigger_id: "<id>"`, `body: {"enabled": false}`

Do NOT delete the trigger — just disable it so it can be re-enabled
later with `/gardener-sync-start`.

## 4. Confirm

Output:
"🌳 gardener sync stopped.
- Local loop: stopped
- Watch window: <stopped (PID ...) | no watch window running>
- Cloud schedule: disabled (not deleted)
- Restart anytime: `/gardener-sync-start`"
