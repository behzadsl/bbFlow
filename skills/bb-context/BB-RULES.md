# BrandBastion FE baseline rules

**app** = `bb_client-facing-app_fe`, **kit** = `bb_ui_kit_fe`. Repo `AGENTS.md`, configs, and neighbouring code win over this file.

## Shared (all BB FE repos)

### Tooling
- **yarn 1** only (`yarn.lock`). Never `npm install` or create `package-lock.json`.
- Node 16.20+. TypeScript, React 18 function components.
- Private packages come from GitHub Packages (`@bbdevcrew:registry`). Never print or commit tokens.
- Never commit `.env.*`, `cypress.env.json`, or secrets. Never invent env values.
- Pre-commit: husky + lint-staged runs Prettier (and `eslint --fix` in the app).

### Formatting (Prettier)
- Double quotes, semicolons, `trailingComma: all`, `arrowParens: avoid`, 2 spaces.
- `printWidth` is **100** in the app and **80** in the kit. Read `.prettierrc.json` if unsure.

### Components
- `export const Name: React.FC<INameProps> = ({ ... }) => { ... }` (kit also uses `forwardRef` + default export; match siblings).
- One folder per component: `Name/Name.tsx`, `Name/Name.module.less`, `Name/Name.types.ts` (kit often uses `Name.type.ts`; match siblings), `Name/index.ts`.
- Helpers in `Name.helpers.ts(x)`, hooks in `useSomething.ts` next to the component.
- Interfaces and types are prefixed with `I` (`IAlert`, `IButtonProps`), and union types end in `Type` (`AlertStatusType`).

### Styles
- Less CSS modules: `import s from "./Name.module.less";`.
- Class names are camelCase with a `bb` prefix: `.bbAIModalWrapper`, `.bbButton`.
- Combine with `classnames`: `const cx = classNames.bind(s);` then `cx(s.bbX, { [s.bbXActive]: isActive }, className)`.
- Reuse kit theme variables and tokens. Do not hardcode colours that exist in the theme.

### i18n
- `react-i18next`: `const { t } = useTranslation();` and keys look like `t("namespace:section:key")` (colon-separated).
- App strings: `src/languages/en_US/<area>.json`. Kit strings: `src/languages/en_US.json`.
- No hardcoded user-facing strings. Add the key to the right JSON file in the same change.

### Import order
Separate the groups with blank lines:
1. `react`, then third-party libraries
2. `@bbdevcrew/bb_ui_kit_fe`, then local components
3. store actions and selectors (`@store/...`), utils
4. styles (`import s from "./X.module.less"`)
5. types (`import { IXProps } from "./X.types"`)

### ESLint
- No `console.log`; `console.warn`, `console.error`, and `console.info` are allowed. No `debugger`.
- `react-hooks/rules-of-hooks` is an error and `exhaustive-deps` a warning. Fix the deps instead of disabling the rule.
- Prefix unused vars and args with `_`. No file extensions in imports. Max line length 100.

## App (`bb_client-facing-app_fe`)

- Path aliases: `@assets`, `@components`, `@containers`, `@pages`, `@store`, `@utils`. Prefer them over deep `../../`.
- UI: prefer `@bbdevcrew/bb_ui_kit_fe` components; use raw `antd` 4 only when the kit has nothing equivalent.
- Feature areas: `src/components/<care|control-panel|insights|login|publish|report|settings|_common>`.
- **State: redux + redux-observable + typesafe-actions.** Each slice lives in `src/store/<slice>/`:
  - `actionTypes.ts`: string constants (`FETCH_X`, `FETCH_X_SUCCESS`, `FETCH_X_FAIL`)
  - `actions.ts`: `createAction(types.X, (payload: T) => payload)()`
  - `epics.ts`: `action$.pipe(filter(isActionOf(actions.x)), switchMap(... ajax<T>({ url, method, body, headers: getAuthAPIHeaders(state$) }) ... map(actions.xSuccess), catchError(err => handleError(err, actions.xFail)))))`
  - `index.ts`: `createReducer(initialState)` with fetching/fetched/failed flags
  - `selectors.ts`, `types.ts`
  - API URLs come from `@utils/paths`. Register the reducer and epics in `src/store/index.ts`.
- Dev server: `yarn start` on port 3003.
- E2E: Cypress in `cypress/e2e/**`. CI runs it on PRs to `develop` only when the last commit subject contains `[e2e]`; PRs to `staging` and `master` always run the matrix.
- Branches: `develop` is the default base. `staging` and `master` are for hotfixes and releases.

## Kit (`bb_ui_kit_fe`)

- Published as `@bbdevcrew/bb_ui_kit_fe` (Rollup, `dist/`). Default branch is `master`.
- Components: `src/components/generic/<Name>/` or `src/components/app-specific/<Name>/`.
- **Every public component, hook, util, or type must be exported from `src/index.ts`** under the matching section comment.
- Add or update a Storybook story in `src/stories/<Name>.stories.tsx` for visual components.
- Runtime libraries shared with apps go in `peerDependencies` (and `devDependencies`) with versions aligned to the app.
- Avoid breaking prop changes. If one is unavoidable, flag it as a Minor or Major bump.
- Release flow (after the PR merges to `master`): pull `master`, run `yarn version`, and `postversion` pushes the tag and triggers publish. Semver:
  - **Major**: new design or architecture (breaking)
  - **Minor**: new page or new basic component
  - **Revision**: refactor, bugfix, or small change
- Then bump consumers: in the app, `yarn add @bbdevcrew/bb_ui_kit_fe@x.y.z` and commit `DEV-123 | Bump @bbdevcrew/bb_ui_kit_fe to x.y.z`.
- Local dev against the app: `yarn link` in the kit, then `yarn link @bbdevcrew/bb_ui_kit_fe` in the app. Undo with `yarn unlink` + reinstall.

## Git and Jira

- Jira: **DEV** and **ITS**. An ITS ticket links to a DEV ticket titled `[SD] <ITS title>`, which holds the assignee and status.
- **Branch:** `<feature|bugfix|hotfix>/<snake_case_summary>_DEV-12345`
  - Hotfix per environment: `hotfix/<summary>_STAGING_DEV-12345`, `hotfix/<summary>_PROD_DEV-12345`
  - No ticket: `..._NO-TASK`. Iterations: `..._DEV-12345_v2`
- **Commit and PR title:** `DEV-12345 | Sentence case summary`
  - Several tickets: `DEV-1, DEV-2 | Summary`. No ticket: `NO TASK | Summary`
  - Add `[e2e]` to the last commit subject when the app PR needs Cypress on `develop`.
- **PR body:** use the repo's `.github/pull_request_template.md`.
- Cross-repo changes (kit + app) need **Related PRs** on both sides.
