# bbFlow

Agent Skills for BB FE repos. State lives in `.cursor/bbflow/` in each repo.

```
/bb-context → /bb-brief + ticket details → /bb-make → /bb-prove → /bb-review
/bb-fix anytime · /bb-kit for UI kit · /bb = status
```

## Commands

| Command | What it does |
|---------|--------------|
| `/bb-context` | Builds `CONTEXT.md` for the repo |
| `/bb` | Status and next step |
| `/bb-brief` | Brief and branch name from the ticket details you paste |
| `/bb-make` | Implements the brief |
| `/bb-prove` | Lint, types, build, UI check |
| `/bb-review` | PR review |
| `/bb-fix` | Bug fix loop |
| `/bb-kit` | UI kit link, release, and bump |

## Rules priority

1. Repo `AGENTS.md`
2. Repo configs
3. `skills/bb-context/BB-RULES.md`

## Install

```bash
npx skills add behzadsl/bbFlow -g -y -a cursor
```

Update with `npx skills update`. Open a new chat, then run `/bb-context` in the repo.

## Files (`.cursor/bbflow/`)

| File | Written by |
|------|------------|
| `CONTEXT.md` | `/bb-context` |
| `BOARD.md` | all |
| `BRIEF.md` | `/bb-brief` |
| `PROVE.md` | `/bb-prove` |
| `REVIEW.md` | `/bb-review` |
