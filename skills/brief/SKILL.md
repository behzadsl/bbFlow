---
name: brief
description: Turn pasted ticket details into .cursor/bbflow/BRIEF.md with a branch name. Use when the user runs /brief or starts a new ticket.
disable-model-invocation: true
---

# /brief: plan the ticket

## Steps

1. Read `.cursor/bbflow/CONTEXT.md`. If missing, run the `/context` steps first.
2. **Ticket details come from the user.** Do not fetch Jira. If they are missing, ask for: key, title, description, acceptance criteria, Figma link. With an ITS key, ask for the linked `[SD]` DEV key. With no ticket, use `NO TASK`.
3. If the goal is still unclear, ask up to **3** short questions.
4. Identify the affected areas. Flag if the change **needs a kit change first**, and whether BE work is pending.
5. Propose a branch name per BB-RULES off the default base. Create or switch branches **only if the user agrees**.
6. Write `.cursor/bbflow/BRIEF.md`:

```markdown
# Brief: DEV-12345 · <title>

## Links
- Jira: DEV-12345 (ITS-… if service desk) · Figma: … · Related PRs: …

## Goal
One paragraph: what success looks like for the user.

## In scope
- …

## Out of scope
- …

## Touch points
- Components / store slice / i18n keys / kit components or exports

## Acceptance
- [ ] … (from the ticket AC; one line each)
- [ ] No ESLint errors, types pass
- [ ] i18n keys added for new strings

## Notes
Assumptions, BE status, kit dependency, risks, open questions.
```

7. Create or update `.cursor/bbflow/BOARD.md`:

```markdown
# Board

## Current
- **Ticket:** DEV-12345 · <title>
- **Branch:** <branch>
- **Base:** develop | master | staging
- **Phase:** briefed
- **Updated:** YYYY-MM-DD
- **Next:** /make

## History
- YYYY-MM-DD briefed · DEV-12345
```

8. Stop. Next step is `/make`.

## Rules

- Keep the brief under about 50 lines.
- Do not write app code in `/brief`.