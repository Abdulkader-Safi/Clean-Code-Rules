# Rule 07: Booleans read as yes/no questions, in the positive

## What it is

Name boolean variables and functions so they read as a question with a yes or no answer: `isActive`, `hasPaid`, `canEdit`, `shouldRetry`, `wasDeleted`. Name them in the positive. `isEnabled`, not `isNotDisabled`.

## Why it works

`if (isVisible)` reads like English. `if (!isNotHidden)` makes the reader do logic in their head, and double negatives are a common source of flipped conditions. A bare noun like `status` or `flag` doesn't say what `true` means.

## Check

Read every `if` aloud. If it doesn't sound like a sentence, rename. Search for `Not` and `No` inside boolean names.

## Bad

```ts
let flag = false;
const disabled = !user.notVerified;
if (!isNotExpired(token)) { ... }
function check(user) { return user.role === 'admin'; }
```

## Good

```ts
let hasRetried = false;
const isVerified = user.emailVerifiedAt !== null;
if (isExpired(token)) { ... }
function isAdmin(user: User): boolean { return user.role === 'admin'; }
```

## Fix

Flip negative names and their call sites together. For complex conditions, pull them into a named boolean (rule 17).

Sources: Robert C. Martin, *Clean Code*, ch. 17 "Avoid negative conditionals" (G29). [Microsoft .NET naming guidelines](https://learn.microsoft.com/en-us/dotnet/standard/design-guidelines/names-of-type-members) (Boolean properties phrased affirmatively, often with `Is`, `Can`, `Has`).

Evidence: convention (official framework guidance, no controlled test).
