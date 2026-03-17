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

### Good commit message examples
- `sense_amp: add sa_core schematic`
- `testbench: add transient sweep for SAE pulse`
- `results: add decision delay vs input differential plot`
- `docs: update lab notes for BSIM3 sweep`

---

## Local PC (to sync latest)

```powershell
cd "C:\path\to\Cadence Repo"
git pull
```

---

## What Is Safe to Commit

| File Type | Safe to Push | Notes |
|---|---|---|
| Schematics (exported) | Yes | Your own work |
| Netlists you wrote | Yes | Your own work |
| OCEAN scripts | Yes | Your own work |
| Simulation results / plots | Yes | Your own work |
| Lab reports / docs | Yes | Your own work |
| README, notes, markdown | Yes | Your own work |
| PDK model files (`.scs`, `.mod`, `.lib`) | **NO** | AMS licensed |
| Cadence install files | **NO** | Cadence licensed |
| Files from `/usr/local/cadence/` | **NO** | Licensed software |
| Files from `/tools/` | **NO** | Licensed software |

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