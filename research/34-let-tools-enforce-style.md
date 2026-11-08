# Rule 34: Let tools enforce style

## What it is

Formatting (indentation, quotes, line length, import order) is decided by a tool, not by a person or a model. Every project has:

- A formatter run on save and in CI: Prettier (JS/TS/Svelte), Laravel Pint (PHP), `dotnet format` / CSharpier (C#), clang-format (C++), Ruff (Python).
- A linter with rules that catch real bugs: ESLint with `typescript-eslint`, Larastan/PHPStan, Roslyn analyzers, clang-tidy.
- The strictest type checking the project can bear: `strict: true` in `tsconfig`, PHPStan level 6 or higher, nullable reference types on, warnings as errors in CI.

When writing code, run the tools and fix what they report before calling a change done. Don't hand-format against the tool, and don't disable a rule inline without a comment saying why.

## Why it works

Google's guide says not to block changes "based only on personal style preferences". A formatter ends that argument entirely. Linters and strict types catch whole classes of bugs (unused variables, unhandled promises, null access) before a human ever reads the code, which frees review time for design.

## Check

Is there a formatter config in the repo? Does CI run the formatter check, the linter and the type checker? Does the change add any `eslint-disable`, `@ts-ignore`, `@phpstan-ignore` or `#pragma warning disable` without a reason?

## Bad

```ts
// @ts-ignore
const total = order.items.reduce((s, i) => s + i.price, 0);
/* eslint-disable */
```

## Good

```ts
// @ts-expect-error: lib types miss the `locale` option added in v4.2; remove after upgrading @types/foo
formatter.format(value, { locale });
```

## Fix

Add the missing tool configs to the project (ask first on an existing project, since turning on a formatter rewrites many files; do it as its own commit, rule 33). Replace `@ts-ignore` with `@ts-expect-error` plus a reason.

Sources: [Google code review guide](https://google.github.io/eng-practices/review/reviewer/looking-for.html). [typescript-eslint: recommended configs](https://typescript-eslint.io/users/configs/). [Laravel Pint](https://laravel.com/docs/pint)

Evidence: strong (official guidance).
