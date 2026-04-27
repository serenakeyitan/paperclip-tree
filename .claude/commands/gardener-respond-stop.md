Stop the gardener-respond loop.

## 1. Stop local loop

`/loop` running `/gardener-respond-loop` is stopped by pressing
`Ctrl+C` or closing the terminal. If running under `/schedule`,
remove the schedule entry.

## 2. Stop any open watch window

`/gardener-respond-watch` records its PID at
`~/.gardener/respond-watch.pid`. Kill it so the tail stops.

```bash
PIDFILE="$HOME/.gardener/respond-watch.pid"
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

## 3. Confirm

Output:
"🌱 gardener-respond stopped.
- Local loop: stop with Ctrl+C in the /loop session
- Watch window: <stopped (PID ...) | no watch window running>"
