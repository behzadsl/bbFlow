# bbFlow

Agent Skills for BB FE repos. State lives in `.cursor/bbflow/` in each repo.

```
/context → /brief + ticket details → /make → /prove → /review
/fix anytime · /kit for UI kit · /bb = status
```

## Commands

| Command | What it does |
|---------|--------------|
| `/context` | Builds `CONTEXT.md` for the repo |
| `/bb` | Status and next step |
| `/brief` | Brief and branch name from the ticket details you paste |
| `/make` | Implements the brief |
| `/prove` | Lint, types, build, UI check |
| `/review` | PR review |
| `/fix` | Bug fix loop |
| `/kit` | UI kit link, release, and bump |

## Rules priority

1. Repo `AGENTS.md`
2. Repo configs
3. `skills/context/BB-RULES.md`

## Install

```bash
npx skills add behzadsl/bbFlow -g -y
```

Update with `npx skills update`. Open a new chat, then run `/context` in the repo.

## Files (`.cursor/bbflow/`)

| File | Written by |
|------|------------|
| `CONTEXT.md` | `/context` |
| `BOARD.md` | all |
| `BRIEF.md` | `/brief` |
| `PROVE.md` | `/prove` |
| `REVIEW.md` | `/review` |
