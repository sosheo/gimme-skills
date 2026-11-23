---
name: gimme-review
description: Review a GitHub pull request against its Linear ticket and the standards of the codebase. Gives a short summary, the key decisions, issues ranked by severity with blockers marked, and a checklist of the ticket requirements. Use when the user types /gimme-review with a PR URL.
argument-hint: <pr-url> [linear-ticket-url-or-id]
disable-model-invocation: true
---

# Gimme review

Review this PR: $ARGUMENTS

The first argument is the PR. The second argument, if present, is the Linear ticket for the PR.

- If no PR is given, ask the user for the PR.
- If no ticket is given, find the ticket in the branch name, the title or the description of the PR.
- If the PR names no ticket, ask the user for the ticket.

Do not change files, check out the PR branch or post to GitHub. The review is for the user only, and the working tree can hold other work.

## What to look for

- **Issues.** Find each issue in the PR.
- **Regressions.** Make sure that you catch each regression. Read the callers and the tests of the changed code, not only the diff.
- **Divergences.** Pay very close attention to each place where the PR diverges from the approaches and standards of the codebase. Compare the PR with similar code in the repo and with the AGENTS.md rules.
- **Ticket requirements.** Check each requirement of the ticket against the PR.

## Output

Write the review in this order.

1. **What the PR delivers.** Write one sentence, two at most.
2. **Key decisions.** Give a bullet list of the decisions that the PR makes or implies.
3. **Issues.** Give a numbered list, ranked by severity, with the most severe issue first.
    - Give the file and the line for each issue.
    - Mark each issue that blocks the merge as `Blocker`.
    - Mark each regression as `Regression`.
    - Mark each divergence as `Divergence`.
4. **Ticket requirements.** Give a bullet checklist with one line for each requirement. Show if the PR resolves the requirement or not.

Example of the format for sections 3 and 4:

```markdown
### Issues

1. **Blocker · Regression** `src/reports/buildReport.ts:88`
   The new guard skips each report that has no rows, so an empty report loses its header.
2. **Divergence** `src/utils/formatDate.ts:12`
   The repo keeps date helpers in `src/lib/date/`. This helper is in `src/utils/`.
3. `tests/reports/buildReport.test.ts:30`
   The test name says that the test checks the title, but no assertion reads the title.

### Ticket requirements

- ✅ A weekly report shows a total for each row: `addRowTotals` writes `row.total`.
- ❌ A daily report is unchanged: see issue 1.
```

## Follow-up: production impact

Do not answer this question in the review. Answer it when the user asks for it, for example with "prod impact?".

Question: if the PR was merged and released today, what would change for production?

If nothing changes until a flag, a setting or a later PR is in place, say so.
