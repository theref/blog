# Project Context

## Vault Connection

This project's planning data lives in the brain vault.
The `.planning/` directory is symlinked from `~/brain/projects/blog/gsd-planning/`.

**Before starting new work:** Search the vault for prior art on the topic.
```bash
# From the vault
cd ~/brain && grep -rl "<topic>" technologies/
```

**Planning:** Use GSD for multi-phase work (`/gsd:discuss-phase`, `/gsd:plan-phase`, `/gsd:execute-phase`).
Use TaskNotes for daily task tracking (not GSD).

**After completing work:** Run `/knowledge-capture` to extract learnings into the vault.

## Vault Location

The brain vault is at `~/brain` (symlink to `/Volumes/BigSSD/brain`).
Project notes: `~/brain/projects/blog/`
Technology knowledge: `~/brain/technologies/`
