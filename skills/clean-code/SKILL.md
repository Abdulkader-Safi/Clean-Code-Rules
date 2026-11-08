---
name: clean-code
description: Use when writing, editing, refactoring or reviewing code in any language or project. Applies 36 researched rules for readable, easy-to-maintain code (naming, functions, control flow, types, errors, comments, structure, tests) and a pre-commit checklist. The project's own conventions always win.
---

# Clean code

## Rule zero: the codebase wins

Before the first edit, open two or three files near the change. Note how files are named, how errors are returned, where tests live and what a typical function looks like. Match that.

When rules clash, this is the order:

1. The project's tooling config (linter, formatter, type checker, editorconfig)
2. The language or framework's official style guide
3. What the surrounding code already does
4. The rules below

Raise a style disagreement as a suggestion to the user. Never "fix" a convention in passing inside an unrelated change.

## How to use the rules

The one-liners below are enough for most edits. When a rule is in play and you need the reasoning and worked examples, open its file. Rule files live in the plugin's `research/` folder, two levels up from this skill's base directory: `<base>/../../research/NN-name.md`. Open only the files you need.

## The rules

Mindset

- 01 Follow the codebase before any rule here: `01-follow-the-codebase-first.md`
- 02 Write for the next reader. "Too complex" means it can't be understood quickly: `02-write-for-the-reader.md`
- 03 Kent Beck's four rules, in order: passes tests, reveals intent, no duplication, fewest elements: `03-four-rules-of-simple-design.md`
- 04 Build only what the task needs now: `04-build-only-what-is-needed.md`

Naming

- 05 Names say what a thing is, in full words. Length fits scope: `05-names-reveal-intent.md`
- 06 One word per concept, the domain's word: `06-one-word-per-concept.md`
- 07 Booleans read as yes/no questions, in the positive (`isActive`, not `notDisabled`): `07-booleans-read-as-questions.md`
- 08 Functions are verbs, types are nouns. `get`, `fetch` and `find` keep distinct meanings: `08-verbs-for-functions-nouns-for-things.md`
- 09 Name your numbers and put units in the name (`timeoutMs`): `09-units-and-magic-numbers.md`
- 10 If it's hard to name, the design is wrong: `10-hard-to-name-means-bad-design.md`
- 37 Naming conventions by language (reference): `37-naming-conventions-by-language.md`

Functions

- 11 Size functions by depth, not line count. Split by meaning; inline pass-throughs: `11-size-functions-by-depth.md`
- 12 Handle edge cases first and return early: `12-guard-clauses-early-return.md`
- 13 Few arguments, never a bare boolean flag. Four or more become an object: `13-few-arguments-no-flag-arguments.md`
- 14 Keep calculations pure and push I/O to the edges: `14-separate-logic-from-side-effects.md`
- 15 One level of detail per function: `15-one-level-of-abstraction.md`
- 16 Don't mutate what you were given: `16-dont-mutate-inputs.md`

Control flow

- 17 Name complex conditions and keep control flow flat: `17-keep-cognitive-complexity-low.md`
- 18 Boring code over clever code. No nested ternaries: `18-boring-over-clever.md`

Data and types

- 19 Parse input once at the boundary, then trust the types: `19-parse-dont-validate.md`
- 20 Give important values their own types. Money is never a float: `20-avoid-primitive-obsession.md`
- 21 Decide where "nothing" is allowed and say so in the type: `21-handle-null-deliberately.md`

Errors

- 22 Fail loudly, catch only where you can act, keep the cause: `22-never-swallow-errors.md`
- 23 Design errors out of existence where you can: `23-design-errors-away.md`

Comments

- 24 Comments explain why; code explains what. Exported functions get interface docs: `24-comments-explain-why.md`

Structure

- 25 Modules should be deep: a simple interface hiding real work: `25-deep-modules-hide-information.md`
- 26 Group files by feature, not by technical type: `26-group-by-feature.md`
- 27 Business logic doesn't import the framework: `27-dependencies-point-inward.md`
- 28 Prefer composition over inheritance: `28-composition-over-inheritance.md`
- 29 One reason to change per file or class: `29-keep-files-and-classes-focused.md`

Duplication and dependencies

- 30 Prefer duplication over the wrong abstraction. Wait for the third copy: `30-rule-of-three-wrong-abstraction.md`
- 31 Reuse what exists before writing anything new. Search first: `31-reuse-before-writing.md`

Tests and changes

- 32 Test behaviour through the public interface, not mocks: `32-test-behaviour-not-implementation.md`
- 33 Keep changes small; tidying goes in its own commit: `33-separate-tidying-from-behaviour.md`
- 34 Let tools enforce style. Run them: `34-let-tools-enforce-style.md`

## Run the tools

Rules 12, 17, 21 and 34 are checked better by tools than by eye. Find the project's formatter, linter, type checker and test command (package.json scripts, composer.json, Makefile, .csproj, pyproject.toml, CI config) and run them before calling a change done. Don't add a new tool or config without asking.

## Before finishing: the AI failure list

Read your full diff and check each item (detail in `35-ai-failure-modes.md`):

1. Wrote a new helper when one already exists
2. Over-engineering: one-implementation interfaces, factories, unasked-for options, generics for one type
3. Defensive noise: try/catch everywhere, null checks the types already rule out, fallbacks that hide bugs
4. Comments that narrate the code
5. Changed unrelated code
6. Ignored project conventions
7. Left dead code, unused imports or commented-out blocks
8. Tests that only test mocks
9. Weakened checks to silence an error (`as any`, `!`, `@ts-ignore`, deleted tests)
10. Placeholder logic presented as finished
11. Called an API or config key that doesn't exist in the installed version
12. Fixed the symptom at one call site instead of the shared root cause

Then run the pre-commit checklist in `36-pre-commit-checklist.md`. Delete what either list catches.
