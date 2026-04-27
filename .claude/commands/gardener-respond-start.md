Start the gardener-respond module (review feedback → fix → learn loop).

## 0. Version check

Read and execute `.claude/commands/gardener-version-check.md` as a
runbook. If it exits, stop here — do not proceed. If it warns but
continues, proceed.

---

Default interval: **30 minutes**.

```bash
/loop 30m /gardener-respond-loop
```

To change interval: `/loop <interval> /gardener-respond-loop`

Stop with: `/gardener-respond-stop`
