---
name: bb-make
description: Implement the current bbFlow brief. Use when the user runs /bb-make or asks to build the briefed ticket.
disable-model-invocation: true
---

# /bb-make: build it

## Steps

1. Read root `AGENTS.md` if present and `.cursor/bbflow/CONTEXT.md`. If CONTEXT is missing, suggest `/bb-context` once, but do not block.
2. Read `.cursor/bbflow/BRIEF.md`. If it is missing, stop and suggest `/bb-brief`.
3. Set the board **Phase** to `making`.
4. **Find two or three similar existing files first** and copy their shape.
5. Implement the smallest change that meets **Acceptance**:
   - **App**
     - UI from `@bbdevcrew/bb_ui_kit_fe` first; raw `antd` only if the kit lacks it
     - Aliases and import-group order
     - Data through the store slice (see BB-RULES); no API calls from components
   - **Kit**
     - Exported from `src/index.ts`, story in `src/stories/`
     - Keep props backwards compatible; note any breaking change in the brief **Notes**
   - **Both**
     - Less module, `bb` classes, `classnames`
     - All user-facing text via `t()`, keys added to `en_US`
     - `I…Props` in `Name.types.ts`
     - No `console.log`, `any` only with a reason, hooks deps correct
6. If the app needs a kit change, make it in the kit repo (or tell the user) and record it in the brief **Notes**.
7. `npx prettier --write` and `npx eslint --fix` on changed files only.
8. Update the board: **Phase** `proving` if ready (Next `/bb-prove`), otherwise stay `making` and name the blocker.

## Rules

- Stay inside In scope. No drive-by refactors or reformatting of untouched files.
- Do not bump dependencies (including the kit) unless the brief says so.
- Do not commit, push, or open PRs unless the user asks.
- Do not claim done before `/bb-prove`.
