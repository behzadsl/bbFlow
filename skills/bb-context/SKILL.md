---
name: bb-context
description: Build or refresh .cursor/bbflow/CONTEXT.md by merging BB-RULES.md with the repo's AGENTS.md and configs. Use when the user runs /bb-context or asks which BB rules apply.
disable-model-invocation: true
---

# /bb-context: BB repo rules

**Priority (highest wins):**

1. Root `AGENTS.md` (or `.cursor/rules/*`)
2. `package.json`, `.eslintrc*`, `.prettierrc.json`, `tsconfig.json`, `.github/`
3. [BB-RULES.md](BB-RULES.md)

## Steps

1. Read [BB-RULES.md](BB-RULES.md).
2. Look for `AGENTS.md`, `CLAUDE.md`, and `.cursor/rules/` in the repo root. `AGENTS.md` is the override.
3. Detect the **kind**:
   - `name` is `@bbdevcrew/bb_ui_kit_fe`, or `rollup.config.js` + `publishConfig`: **kit**
   - depends on `@bbdevcrew/bb_ui_kit_fe` and has `src/store` or `scripts/start.js`: **app**
   - otherwise **other** (apply only the Shared and Git/Jira sections)
4. Verify the baseline against reality: scripts, Prettier `printWidth`, aliases in `tsconfig.json`, default branch (`git symbolic-ref refs/remotes/origin/HEAD`), PR template, installed kit version. The repo wins; note differences.
5. Make sure `.cursor/bbflow/` is git-ignored (`git check-ignore -q .cursor/bbflow/BOARD.md`). If not, append `.cursor/bbflow/` to `.git/info/exclude`. Never edit the tracked `.gitignore` for this.
6. Write `.cursor/bbflow/CONTEXT.md`, copying in the BB-RULES.md sections for this kind:

```markdown
# Context

## Source
- **Override:** AGENTS.md | none
- **Kind:** app | kit | other
- **Repo:** <package name> @ <version>
- **Default base:** develop | master | …
- **Kit version used:** x.y.z | n/a

## Commands
| Goal | Command |
|------|---------|
| dev | … |
| lint | … |
| types | … |
| build | … |
| e2e / stories | … |

## Conventions
<Shared + kind-specific sections from BB-RULES.md, adjusted to this repo>

## Git and Jira
<Git and Jira section from BB-RULES.md, with the real default base>

## Prove defaults
- …

## Notes
- Deviations from BB-RULES, gotchas
```

   Prove defaults by kind:
   - **app:** `npx eslint <changed files>`, `npx tsc --noEmit -p .`, manual UI check via `yarn start` (port 3003); `yarn build` for wide changes; Cypress only when asked or when `[e2e]` is needed.
   - **kit:** `npx eslint <changed files>`, `yarn build`, Storybook check for visual components, `src/index.ts` exports present.
7. Make sure `.cursor/bbflow/BOARD.md` exists (Phase `none`, Next `/bb-brief`).
8. Reply briefly: kind, override on/off, default base, next step `/bb-brief`.

## Rules

- Never invent secrets, tokens, or env values. Do not read `.env*` contents.
- Do not implement features here.
