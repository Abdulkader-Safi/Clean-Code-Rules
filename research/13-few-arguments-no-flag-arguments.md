# Rule 13: Keep arguments few and never pass a bare boolean flag

## What it is

Up to three positional arguments is fine. Past that, or when arguments of the same type sit next to each other, pass a single object with named fields. Never pass a bare `true` or `false` that changes what the function does. Split it into two functions, or use a named option or an enum.

## Why it works

`createUser('Sara', 'sara@x.com', true, false, 30)` can't be read without opening the definition, and swapping two strings compiles fine. qntm, otherwise a sharp critic of *Clean Code*, agrees that "unadorned Boolean flags are opaque". Named fields make the call site self-documenting and let you add an optional field later without touching every caller.

## Check

Look at each call site in the change. Can you tell what every argument means without hovering? Any literal `true`, `false` or `null` passed positionally is a red flag.

## Bad

```ts
sendEmail(user, template, true, false);
function render(items: Item[], compact: boolean) { if (compact) { ... } else { ... } }
function transfer(from: string, to: string, amount: number) // easy to swap from/to
```

## Good

```ts
sendEmail({ to: user, template, trackOpens: true, attachInvoice: false });
function renderCompactList(items: Item[]) { ... }
function renderFullList(items: Item[]) { ... }
function transfer({ fromAccount, toAccount, amountFils }: Transfer) { ... }
```

In C# or Python, named arguments do the same job: `Transfer(fromAccount: a, toAccount: b, amountFils: 500)`.

## Fix

Introduce a parameter object, or split the function at the flag. If the two branches share most of their code, keep one private function and expose two clearly named public ones.

Sources: Robert C. Martin, *Clean Code*, ch. 3 "Flag arguments". [qntm review](https://qntm.org/clean). Martin Fowler, *Refactoring*, "Remove Flag Argument" and "Introduce Parameter Object".

Evidence: convention (rare point of agreement between the book and its critics).
