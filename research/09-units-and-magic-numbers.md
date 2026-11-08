# Rule 09: Name your numbers and put units in the name

## What it is

A raw number in logic (`86400`, `0.15`, `3`) is a "magic number": the reader has to guess what it means. Give it a named constant. When a value has a unit, put the unit in the name: `timeoutMs`, `maxUploadBytes`, `priceFils`, `durationSeconds`.

Exceptions: `0`, `1`, `-1`, `100` for percentages, and numbers whose meaning is obvious in place (`array.length - 1`).

## Why it works

`setTimeout(retry, 30)` could mean 30 ms or 30 seconds, and the difference is a bug. Named constants also put a value that changes (a tax rate, a limit) in one place instead of six. Mixing currency units (dinars vs fils, dollars vs cents) is a classic money bug; the unit in the name makes it visible.

## Check

Search the change for numeric literals other than 0 and 1. Each one should either be obvious in place or come from a named constant. Every duration or money value should say its unit.

## Bad

```ts
if (Date.now() - session.createdAt > 1800000) logout();
const total = subtotal * 1.15;
await sleep(5);
```

## Good

```ts
const SESSION_LIFETIME_MS = 30 * 60 * 1000;
const KUWAIT_VAT_RATE = 0; // Kuwait has no VAT as of 2026
const RETRY_DELAY_SECONDS = 5;

if (Date.now() - session.createdAtMs > SESSION_LIFETIME_MS) logout();
await sleep(RETRY_DELAY_SECONDS * 1000);
```

## Fix

Pull the literal into a constant next to where it's used, or into the module's config if it changes per environment. Where the language allows, use a type for the unit (`Duration`, `Money`) instead of a bare number (rule 20).

Sources: Robert C. Martin, *Clean Code*, ch. 17 (G25 "Replace magic numbers with named constants"). Martin Fowler, *Refactoring* (2nd ed., 2018), "Replace Magic Literal".

Evidence: convention.
