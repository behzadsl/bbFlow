---
name: brief
description: Turn a Jira ticket or idea into .cursor/bbflow/BRIEF.md with a branch name. Use when the user runs /brief or starts a new ticket.
disable-model-invocation: true
---

# /brief: plan the ticket

## Steps

1. Read `.cursor/bbflow/CONTEXT.md`. If missing, run the `/context` steps first.
2. **Get the ticket:**
   - If the user gave a key and a Jira/Atlassian MCP is available (check `GetDynamicTools` with pattern `jira|atlassian`), fetch summary, description, acceptance criteria, status, assignee, and `issuelinks`. Use `searchJiraIssuesUsingJql`, not Rovo.
   - **ITS ticket:** follow `issuelinks` to the linked DEV ticket (usually titled `[SD] <ITS title>`) and brief from the DEV ticket. Mention both keys.
   - With no MCP, ask the user to paste the ticket text or Figma link. With no ticket at all, use `NO TASK`.
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
- [ ] … (from the Jira AC; one line each)
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
- Do not change Jira (transition, comment, assign) unless the user asks.
