---
name: gimme-response
description: Assess the comments on a GitHub pull request. For each comment, says if it is still an issue, if it is valid and what the fix is. Looks at all comments unless the user names who posted them or when. Use when the user types /gimme-response with a PR URL.
argument-hint: <pr-url> [who posted | when posted]
disable-model-invocation: true
---

# Gimme response

Look at the comments on this PR: $ARGUMENTS

The first argument is the PR. If no PR is given, ask the user for the PR.

Do not change code or post replies to GitHub. The user decides what to do with each answer.

## Which comments to use

Use all the comments on the PR. If the arguments say who posted the comments, or when, use only those comments.

- `/gimme-response <pr-url>` uses all the comments.
- `/gimme-response <pr-url> by <author>` uses only the comments from that author.
- `/gimme-response <pr-url> since Tuesday` uses only the comments from Tuesday or later.

A PR holds three types of comment. Get all three.

- Conversation comments: `gh pr view <pr-url> --json comments`
- Review summaries: `gh pr view <pr-url> --json reviews`
- Inline review comments: `gh api repos/<owner>/<repo>/pulls/<number>/comments --paginate`

`gh pr view` does not return the inline review comments.

Treat an inline comment and its replies as one comment.

## What to answer for each comment

Answer these questions in order. Stop at the first "no".

1. **Is it still an issue?** Other changes further up the stack can remove the issue. Check the current code of the PR, and of each PR that builds on it.
2. **Is it valid?** Check the claim of the comment against the code.
3. **What is the fix?** Describe the fix, with the file and the line.

To find the PRs further up the stack, list the PRs whose base is the head branch of this PR:

```bash
gh pr list --repo <owner>/<repo> --base <head-branch> --state all
```

Do this again for each PR that you find.

## Output

Give a numbered list with one item for each comment, in the order that the comments were posted. Each item gives:

- the person or bot that posted the comment, and the location (`file:line`, or `conversation`)
- the content of the comment, in one line
- the verdict: `Not an issue now`, `Not valid` or `Valid`
- the reason for the verdict
- the fix, if the verdict is `Valid`

Some comments raise nothing to assess, such as an approval or a bot status message. Do not give these comments an item. Name them in one closing line.
