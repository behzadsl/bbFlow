---
name: kit
description: Link, release, or bump @bbdevcrew/bb_ui_kit_fe. Use when the user runs /kit or asks to link, release, or bump the UI kit.
disable-model-invocation: true
---

# /kit: UI kit release and bump

Modes: **link**, **release**, **bump**.

## Link (local dev)

1. In the kit repo: `yarn link` (first time only), then `yarn dev`.
2. In the app: `yarn link @bbdevcrew/bb_ui_kit_fe`, then `yarn start`.
3. To undo in the app: `yarn unlink @bbdevcrew/bb_ui_kit_fe`, remove `node_modules`, and run `yarn install`.
4. Note in the app's `.cursor/bbflow/BRIEF.md`: "depends on kit change <branch/PR>".

## Release (kit repo)

1. Confirm the kit PR is **merged to `master`**, otherwise stop.
2. Pick the semver level and give a one-line reason:
   - **Major:** new design or architecture, breaking
   - **Minor:** new page or new basic component
   - **Revision:** refactor, bugfix, or small change
3. Run **only with explicit user approval**:

```bash
git checkout master && git pull
yarn version --patch   # or --minor / --major
```

4. Report the new version. Update the board History.

## Bump (app repo)

1. Branch: `bugfix/bump_ui_kit_to_<x_y_z>_DEV-12345`.
2. `yarn add @bbdevcrew/bb_ui_kit_fe@<x.y.z>` (exact version, `yarn.lock` updated).
3. `npx tsc --noEmit -p .` and a UI check of affected screens.
4. Commit and PR title: `DEV-12345 | Bump @bbdevcrew/bb_ui_kit_fe to x.y.z`. Link the kit PR under **Related PRs**.

## Rules

- Never run `yarn version`, publish, or push tags without explicit approval.
- Never print or commit GitHub Packages tokens.
- One kit version per bump PR; list the kit changes it pulls in.
