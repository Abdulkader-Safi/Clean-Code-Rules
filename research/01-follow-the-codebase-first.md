# Rule 01: Follow the codebase before any rule here

## What it is

Before writing code in an existing project, read how it already does things: naming, folder layout, error handling, test style, which libraries it uses. Match that. The rules in this folder only decide what the codebase hasn't already decided.

Priority order when rules clash:

1. The project's own lint, formatter and type-checker config
2. The language or framework's official style guide (PSR/PER for PHP, Microsoft's .NET guidelines, the Svelte or Laravel docs)
3. What the surrounding code already does
4. The rules in this folder

## Why it works

Google's code review guide says it directly: "If no other rule applies, the author should maintain consistency with the existing code." A file with two naming styles makes every reader stop and wonder whether the difference means something. One consistent style that's slightly worse beats a mix of good and better.

## Check

Before the first edit in a project, open two or three files near the change and answer: how are files named, how are errors returned, where do tests live, what does a typical function look like? If the answer differs from these rules, the codebase wins.

## Bad

The project returns `Result<T, E>` objects everywhere. The new function throws exceptions because "exceptions are cleaner".

## Good

The new function returns `Result<T, E>` like its neighbours. If exceptions really are better, that goes in a separate note to Safi, not into one file.

## Fix

Raise a style disagreement as a suggestion. Never "fix" the convention in passing inside an unrelated change.

Source: [Google engineering practices: what to look for in a code review](https://google.github.io/eng-practices/review/reviewer/looking-for.html)

Evidence: strong (official engineering guidance from a large org).
