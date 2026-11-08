# Rule 11: Size functions by depth, not by line count

## What it is

A good function hides a lot of work behind a simple signature. Ousterhout calls this "deep". Split a function when a piece of it is a separate idea with its own clear name, or when it's reused. Don't split just to hit a line count.

This is the most contested rule in clean code. *Clean Code* says functions should be "two, three, or four lines long". Ousterhout argues this produces "teeny-tiny methods that suffer from both shallow interfaces and entanglement": to understand one, you read all of them. In their 2025 public debate, Martin wrote that they "both value decomposition, and we both avoid entanglement; but we disagree on the relative weighting".

The working position for this skill:

- No hard line limit. Treat 40 to 60 lines as a prompt to look, not a rule.
- Extract a block when it has a name that is clearer than its code, or when two callers need it.
- Don't extract a block if understanding the caller now means jumping into the helper anyway ("conjoined methods").
- A helper called once, with a name that adds nothing, gets inlined back.

## Why it works

Ousterhout measured a concrete cost in the debate: splitting the prime generator's loops into separate methods made it 3 to 4 times slower until Martin fixed it. The bigger cost is reading: eight two-line private methods called once each turn a 30-line algorithm into a scavenger hunt. qntm's review of *Clean Code* makes the same point with the book's own FitNesse example, split into fifteen private methods, many called once.

## Check

For each extracted helper: does the name tell you enough that you don't need to read its body? Is it called more than once, or does it hide a real detail? If both answers are no, inline it.

## Bad

```ts
function isEligible(user: User) { return hasAge(user) && hasCountry(user); }
function hasAge(user: User) { return checkAge(user.age); }
function checkAge(age: number) { return age >= MIN_AGE; }
function hasCountry(user: User) { return checkCountry(user.country); }
function checkCountry(country: string) { return SUPPORTED.includes(country); }
```

## Good

```ts
function isEligible(user: User): boolean {
  return user.age >= MIN_AGE && SUPPORTED_COUNTRIES.includes(user.country);
}
```

## Fix

Inline pass-through helpers. For long functions, extract the steps that are separate ideas (parsing, a calculation, a network call) and leave the rest together.

Sources: [Ousterhout and Martin debate, aposd-vs-clean-code](https://github.com/johnousterhout/aposd-vs-clean-code). [qntm: It's probably time to stop recommending Clean Code](https://qntm.org/clean). John Ousterhout, *A Philosophy of Software Design*, ch. 4 "Modules should be deep".

Evidence: contested (two respected authors disagree on the weighting; neither side has controlled data).
