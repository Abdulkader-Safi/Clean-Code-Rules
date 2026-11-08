# Rule 30: Prefer duplication over the wrong abstraction

## What it is

Don't extract a shared function the second time you see similar code. Wait until the third time, and until you can see what actually varies (the "rule of three"). Two blocks that look alike today may change for different reasons tomorrow.

When an existing shared function has grown a pile of parameters and `if` branches to serve different callers, it's the wrong abstraction. Undo it.

DRY is about knowledge, not text. The original definition (Hunt and Thomas): "Every piece of knowledge must have a single, unambiguous, authoritative representation within a system." A tax rate in three places is a DRY violation. Two loops that happen to look alike are not.

## Why it works

Sandi Metz: "duplication is far cheaper than the wrong abstraction." She describes how it goes wrong: someone extracts shared code, a new requirement almost fits, so a parameter and a conditional get added, then another, until nobody understands it and sunk cost stops anyone from removing it. Kent C. Dodds calls the fix AHA, "avoid hasty abstractions": optimise for change first.

## Check

Does a shared function take a boolean or "mode" parameter that switches between callers' needs? Do its callers each use only part of it? Is the abstraction being created for exactly two similar call sites?

## Bad

```ts
function formatAddress(a: Address, forInvoice: boolean, forShippingLabel: boolean, isKuwait: boolean, short?: boolean) {
  // 60 lines of branches, each caller uses a different 15
}
```

## Good

```ts
function formatInvoiceAddress(address: Address): string { ... }
function formatShippingLabel(address: KuwaitAddress): string[] { ... } // block, street, building, area
```

## Fix

Metz's recipe: "the fastest way forward is back". Inline the abstraction back into every caller, delete the branches each caller doesn't use, then look again for real duplication and extract only that.

Sources: [Sandi Metz: The Wrong Abstraction (2016)](https://sandimetz.com/blog/2016/1/20/the-wrong-abstraction). [Kent C. Dodds: AHA Programming](https://kentcdodds.com/blog/aha-programming). Andrew Hunt and David Thomas, *The Pragmatic Programmer* (1999). Martin Fowler, *Refactoring*, "Rule of Three".

Evidence: convention (very widely cited; no controlled test).
