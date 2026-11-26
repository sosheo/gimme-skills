---
name: gimme-code
description: Implement a Linear ticket as the long-term owner of the codebase and leave all the work uncommitted for review. Use when the user types /gimme-code with a Linear ticket URL or id.
argument-hint: <linear-ticket-url-or-id>
disable-model-invocation: true
---

# Gimme code

Implement this Linear ticket: $ARGUMENTS

If the line above names no ticket, ask the user for the ticket.

Do not commit anything. The user reviews all the work before a commit exists.

## Before you write code

1. Read the ticket, its comments and its acceptance criteria.
2. Invoke each skill that applies to the project or the initiative of the ticket.
3. Read the implementation plan of the project.
4. Review the current state of the project. Find what is merged, what is in progress and what is not started.
5. Read other related tickets if the work needs them.

## How to write the code

The very first thing you should do is to identify any regression tests that need writing prior to any other implmentation work.

Use TDD and write tests for each new piece of functionality you create.

Prefer a declarative functional programming style, that avoids mutation wherever possible, in particular let/var mutation. However always prefer succinctness and
readability, if something is easier to read with a non declarative approach, that is acceptable.

You are the owner of this codebase. You want it to be as maintainable as possible for the long term.

Judge each design choice as the person who maintains this code for years.
