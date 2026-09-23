---
name: bb
description: Show bbFlow status from .cursor/bbflow/BOARD.md and suggest the next bb command. Use when the user runs /bb or asks for bbFlow status.
disable-model-invocation: true
---

# /bb: status

## Steps

1. Look for `.cursor/bbflow/BOARD.md`. If it is missing, say bbFlow has not started here and suggest `/bb-context`.
2. Read the board, plus `BRIEF.md` and `CONTEXT.md` in the same folder if present.
3. Check `git branch --show-current` and `git status --short`.
4. Reply with a short status:
   - **Ticket:** `DEV-12345`: title (or `NO TASK`)
   - **Repo:** app | kit | other (from CONTEXT), plus `AGENTS.md` override on/off
   - **Branch:** current branch, and whether it matches `<feature|bugfix|hotfix>/<snake_case>_DEV-12345`
   - **Phase:** briefed | making | proving | proved | reviewed
   - **Blockers:** only if listed
   - **Next:** one command
5. Do not invent a plan or edit code.

## Phase hints

| Board phase | Next |
|-------------|------|
| none / missing CONTEXT | `/bb-context` |
| none | `/bb-brief` + ticket details |
| briefed | `/bb-make` |
| making | finish `/bb-make`, then `/bb-prove` |
| proving | finish `/bb-prove` (or `/bb-fix`) |
| proved | `/bb-review` |
| reviewed, with blockers | `/bb-make` or `/bb-fix` |
| reviewed (kit) | `/bb-kit` to release and bump consumers |
| reviewed | done, or `/bb-brief` for the next ticket |
