---
name: bb-fix
description: Find and fix a bug's root cause from pasted ticket details, then hand off to /bb-prove. Use when the user runs /bb-fix, reports broken behaviour, or pastes a bug ticket.
disable-model-invocation: true
---

# /bb-fix: debug

## Steps

1. Read `AGENTS.md` if present and `.cursor/bbflow/CONTEXT.md`.
2. **Ticket details come from the user.** Do not fetch Jira. If missing, ask for: key (DEV, not ITS), expected vs actual, repro steps, console/network errors, environment.
3. With no brief, create a minimal `BRIEF.md` (Goal, Acceptance) and suggest a `bugfix/` or `hotfix/` branch.
4. Form 1–3 hypotheses and check the cheapest evidence first:
   - **Data:** epic `catchError` swallowing errors, wrong `@utils/paths` URL, payload shape, reducer flags (`fetching`/`fetched`/`failed`) not reset, selector memoization
   - **UI:** installed kit version vs kit source, `classnames` conditions, Less specificity, antd 4 overrides
   - **i18n:** missing key or wrong namespace
   - **Recent change:** `git log -p -- <file>` for related `DEV-` commits
5. Fix the **root cause**, not the symptom, unless the user asks for a temporary guard. If the bug is in the kit, fix it there and note `/bb-kit` is needed.
6. Log it in `.cursor/bbflow/BOARD.md` (or `FIX.md` if longer): symptom, root cause, fix.
7. Set board **Next** to `/bb-prove`, and stop unless the user says to continue.

## Rules

- No new features under `/bb-fix`. If the behaviour was never defined, route to `/bb-brief`.
- Suggest a regression check for `/bb-prove` (manual steps or a Cypress spec).
