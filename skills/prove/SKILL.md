---
name: prove
description: Verify the current bbFlow change against the brief acceptance. Use when the user runs /prove or wants to confirm a change works.
disable-model-invocation: true
---

# /prove: check the change

## Steps

1. Read `AGENTS.md` if present, `.cursor/bbflow/CONTEXT.md`, and `.cursor/bbflow/BRIEF.md` (required).
2. Set the board **Phase** to `proving`.
3. List changed files: `git diff --name-only <base>...HEAD` plus the working tree.
4. Run the checks for the kind:
   - **app:** `npx eslint <changed ts/tsx>` (zero errors; report warnings introduced by the change), `npx tsc --noEmit -p .`, and `yarn build` only for wide or config changes.
   - **kit:** `npx eslint <changed ts/tsx>`, `yarn build`, a check that new public items are exported from `src/index.ts`, and `yarn test` if tests exist for the touched area.
   - **i18n:** every new `t("…")` key exists in the `en_US` JSON; no hardcoded UI strings in the diff.
5. UI: if `yarn start` (app) or `yarn storybook` (kit) can run, offer a browser check; otherwise list manual steps. Never create or fill `.env.development`.
6. Cypress (app): not by default. Suggest `yarn cy:run --spec <spec>` or an `[e2e]` commit for core flows.
7. Write `.cursor/bbflow/PROVE.md`:

```markdown
# Prove: DEV-12345

## Result
pass | fail | partial

## Checks
- [x] … (how it was verified)
- [ ] … (why it was not)

## Commands run
```
command → result
```

## Gaps
- …
```

8. Update the board: **pass** → Phase `proved`, Next `/review`. **fail or partial** → Phase `making`, Next the fix, then `/prove` (or `/fix` if the cause is unclear).

## Rules

- Never mark a command as passed if it was not run.
- Pre-existing lint or type errors outside the diff are noted, not fixed.
- Fix only what is needed to pass; no polishing here.
