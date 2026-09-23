---
name: review
description: Review the current diff against BB rules and write .cursor/bbflow/REVIEW.md. Use when the user runs /review or wants a PR review.
disable-model-invocation: true
---

# /review: PR review

## Steps

1. Read `AGENTS.md`, `.cursor/bbflow/CONTEXT.md`, `BRIEF.md`, `PROVE.md` if present, and `.github/pull_request_template.md`.
2. **Ask what to compare against** unless the user already said: default base from CONTEXT, working tree only, or a PR number (`gh pr diff <n>`). Never pick silently.
3. Review, in this order:
   - **Correctness** against the brief and Jira AC
   - **Bugs and edge cases:** loading, empty, and error states; epics that never dispatch a `…Fail`; stale selectors; missing hook deps; unsubscribed observables
   - **Security:** tokens or secrets, env values, `dangerouslySetInnerHTML`, unsafe URLs
   - **Contracts:** API payload shape, breaking kit props (flag semver level), missing `src/index.ts` exports
   - **BB-RULES** violations
   - **Scope:** drive-by refactors, unrelated formatting churn, unnecessary complexity
   - **PR hygiene:** branch name, `DEV-12345 | …` title, `[e2e]` if needed, related kit/app PRs
4. Write `.cursor/bbflow/REVIEW.md`:

```markdown
# Review: DEV-12345

## Base
- **Compared against:** … (chosen by the user)
- **Range:** …

## Verdict
approve | approve-with-nits | request-changes

## Summary
2–4 sentences.

## Findings
### Blockers
- [ ] file:line: issue, and why it matters

### Should fix
- [ ] file:line: issue

### Nits
- [ ] file:line: optional polish

## BB PR checklist
- [ ] Descriptive title with the correct Jira key
- [ ] Import order, naming conventions, high-level logic self-reviewed
- [ ] No ESLint errors or warnings
- [ ] UI self-check done, AC met (or comments left in Jira/Figma)
- [ ] Smoke / e2e tests if core flows are affected

## What looks good
- …
```

5. Update the board: **Phase** `reviewed`; Next `/make` or `/fix` if there are blockers, otherwise `/kit` for kit repos or done.
6. In chat, show the verdict and blockers.

## Rules

- Cite files and lines. Do not invent issues.
- Do not commit, push, approve, or merge PRs.
- If the chosen base has no diff, say so and stop.
