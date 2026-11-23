---
name: gimme-commits
description: Propose a list of granular commits for the uncommitted work, as a markdown table of the files and a brief commit message for each commit. Use when the user types /gimme-commits.
disable-model-invocation: true
---

# Gimme commits

Give the user a list of commits to make for the uncommitted work.

Do not stage or commit anything. The list is a proposal, and the user decides what to commit.

## Scope

1. Find each repo that holds uncommitted work from this session. If the session did no work, use the repo of the current directory.
2. Run `git status` and `git diff` in each repo.
3. Put each changed file in one commit. Include the untracked files.
4. Keep a change out of the commits if it is not part of the work. Name it below the table.

## How to split the commits

- Make granular commits. Use one file for each commit when possible.
- Put several files in one commit only when a split makes no sense. An example is a method rename across several related files.
- Put the commits in an order that works. A commit must not need code from a later commit.
- Write a brief commit message for each commit. Follow the commit message rules of the repo.

## Output

Give one markdown table for each repo, with the files and the commit message for each commit.

Example:

```markdown
| # | Files | Commit message |
| --- | --- | --- |
| 1 | `src/modules/tax/itemTaxLine.ts` | `feat: calculate the tax for each line` |
| 2 | `src/test/modules/tax/itemTaxLine.test.ts` | `test: cover the tax for each line` |
| 3 | `src/modules/order/placeOrder.ts`<br>`src/modules/order/updateOrder.ts` | `refactor: rename getTax to getLineTax` |
```
