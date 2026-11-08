# Rule 31: Reuse what exists before writing anything new

## What it is

Before writing a helper, check in this order and stop at the first one that works:

1. Does this need to exist at all?
2. Is there already a helper, type or pattern for it in this codebase?
3. Does the language's standard library do it? (`Intl`, `structuredClone`, `Object.groupBy`, LINQ, Laravel's `Str` and `Collection`)
4. Does the platform do it? (`<input type="date">`, CSS instead of JS, a database constraint instead of app code)
5. Does an already-installed dependency do it?
6. Only then write new code, as little as works.

Don't add a new dependency for something a few lines can do. When a dependency is worth adding, check that it's maintained, check its size and licence, and say why in the commit.

## Why it works

GitClear's analysis of 211 million changed lines found copy-pasted code blocks (5+ duplicated lines) rose eightfold during 2024, and 2024 was the first year copy-pasted lines outnumbered moved (refactored) lines. They link this to AI coding assistants, which tend to write a fresh helper instead of finding the existing one a few files away. Every duplicate is a second place to fix the next bug. Every new dependency is supply-chain risk and upgrade work.

## Check

Before writing a helper, search the codebase for its key verb and noun (`formatDate`, `slugify`, `money`). Check `package.json` / `composer.json` / `.csproj` for a library that already does it.

## Bad

```ts
// src/features/orders/utils.ts
function formatCurrency(n: number) { return 'KWD ' + n.toFixed(3); }
// meanwhile src/shared/money.ts already has formatKwd(), used in 40 places
```

```bash
npm install left-pad is-even moment   # for one call each
```

## Good

```ts
import { formatKwd } from '@/shared/money';
const startOfDay = new Date(date.toDateString());
const label = new Intl.DateTimeFormat('ar-KW', { dateStyle: 'medium' }).format(date);
```

## Fix

Delete the duplicate and point callers at the existing helper. If the existing one is slightly wrong for the new case, fix it there (with its tests) instead of forking it.

Sources: [DevClass on GitClear's 2025 AI code quality report](https://devclass.com/2025/02/20/ai-is-eroding-code-quality-states-new-in-depth-report/). [LeadDev: How AI generated code accelerates technical debt](https://leaddev.com/software-quality/how-ai-generated-code-accelerates-technical-debt)

Evidence: vendor (GitClear sells code analytics; large dataset, method summarised in the report).
