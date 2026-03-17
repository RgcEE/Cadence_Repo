# Daily Workflow — Cadence Repo

> [!NOTE]
> The workflow is based on work being down from University server and then pulling commits to local pc.

## Before Starting Work

```bash
cd ~/Cadence_Repo
git pull
```

Always pull first in case you pushed from local PC.

---

## After Finishing Work

```bash
cd ~/Cadence_Repo
git status          # see what changed
git add .
git commit -m "brief description of what was done"
git push
```

### Commit Types

| Type | Use for | Example |
|------|---------|---------|
| `feat` | new feature | `feat(module01): add CMOS inverter VTC simulation` |
| `fix` | bug fix | `fix(module01): correct model library load order for SFE-2001` |
| `refactor` | internal code revision | `refactor(module01): restructure netlist for readability` |
| `style` | formatting / naming | `style(module01): rename output node to Vout` |
| `docs` | documentation | `docs(module01): add noise margin analysis to inverter notes` |
| `chore` | maintenance / tooling | `chore: update .gitignore to exclude .ahdlSimDB` |
| `perf` | performance improvement | `perf(module01): tighten sweep step for better Vm resolution` |

---

## Local PC (to sync latest)

```powershell
cd "C:\path\to\Cadence Repo"
git pull
```

---

## What the .gitignore Already Blocks

These are excluded automatically — do not manually add them:

```
*.log
*.cdslck
*.swp
CDS.log
.simrc
*.scs
*.mod
*.lib
*.raw
*.ahdlSimDB
```

---

## Rule of Thumb

> If a file came from the PDK install or Cadence tools directory, do not push it.
> If a file is something you created or wrote, it is safe to push.

---

## Quick Reference

| Action | Command |
|---|---|
| Check status | `git status` |
| Stage all changes | `git add .` |
| Commit | `git commit -m "message"` |
| Push to GitHub | `git push` |
| Pull latest | `git pull` |
| See commit history | `git log --oneline` |