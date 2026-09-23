---
name: bb
description: Show bbFlow status from .cursor/bbflow/BOARD.md and suggest the next bb command. Use when the user runs /bb or asks for bbFlow status.
disable-model-invocation: true
---

# /bb: status

## Steps

1. Look for `.cursor/bbflow/BOARD.md`. If it is missing, say bbFlow has not started here and suggest `/context`.
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
| none / missing CONTEXT | `/context` |
| none | `/brief` + ticket details |
| briefed | `/make` |
| making | finish `/make`, then `/prove` |
| proving | finish `/prove` (or `/fix`) |
| proved | `/review` |
| reviewed, with blockers | `/make` or `/fix` |
| reviewed (kit) | `/kit` to release and bump consumers |
| reviewed | done, or `/brief` for the next ticket |
