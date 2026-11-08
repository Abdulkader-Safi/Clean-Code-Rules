# Rule 33: Keep changes small and separate tidying from behaviour

## What it is

Kent Beck's *Tidy First?* splits every change into two kinds:

- Structure changes (tidyings): rename, extract, inline, move, reorder. Behaviour stays exactly the same.
- Behaviour changes: new features, bug fixes.

Never mix the two in one commit. Tidy first if it makes the behaviour change easier, then make the behaviour change. Keep each change small enough to review in one sitting.

Scope rule for an agent: change only what the task needs. No reformatting of untouched lines, no drive-by renames in other files, no "while I was here" refactors. If something nearby is worth tidying, mention it, don't do it.

## Why it works

A diff that mixes a rename across 30 files with a three-line bug fix hides the bug fix. Reviewers can approve a pure structure change quickly ("does it still behave the same?") and focus their attention on the behaviour change. Google's guide asks for major reformatting to go in its own change, separate from functional changes. Small diffs are also easy to revert.

## Check

Does the diff contain lines that have nothing to do with the task? Does one commit both move code and change what it does? Would a reviewer need more than about 15 minutes for it?

## Bad

One commit titled "fix delivery fee" that renames `Order` to `Purchase` across the codebase, reformats 12 files and changes the fee calculation.

## Good

```
commit 1: refactor: extract deliveryFeeFils() from checkout (no behaviour change)
commit 2: fix: charge delivery for orders under 10 KWD, not 5 KWD
```

## Fix

Split mixed changes with `git add -p`. Drop unrelated edits. Leave a note for anything worth tidying later.

Sources: Kent Beck, *Tidy First? A Personal Exercise in Empirical Software Design* (O'Reilly, 2023). [Henrik Warne's review of Tidy First?](https://henrikwarne.com/2024/01/10/tidy-first/). [Google code review guide](https://google.github.io/eng-practices/review/reviewer/looking-for.html)

Evidence: convention (official guidance plus practitioner book).
