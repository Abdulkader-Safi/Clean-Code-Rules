# Rule 03: Use Kent Beck's four rules, in order

## What it is

Kent Beck's rules of simple design, in priority order. Code is simple when it:

1. Passes the tests
2. Reveals intention
3. Has no duplication
4. Has the fewest elements

When two rules pull against each other, the higher one wins. Working code beats readable code. Readable code beats DRY code. Everything else (extra classes, layers, config) has to earn its place by serving the first three.

## Why it works

It's a short list that settles most design arguments. "Should I extract this?" becomes "does extracting it reveal intention or remove real duplication? If neither, don't." Martin Fowler notes that elements added for speculative flexibility "often make a system harder to modify". Beck also said that in rare conflicts, such as in tests, empathy for the reader outweighs the technical rules.

## Check

For every new class, interface, file, parameter or config value in a change, name which of rules 1 to 3 it serves. If you can't, delete it.

## Bad

An `IPriceCalculator` interface, a `PriceCalculatorFactory` and a `DefaultPriceCalculator` for a single function that multiplies quantity by unit price.

## Good

```ts
export function lineTotal(quantity: number, unitPriceCents: number): number {
  return quantity * unitPriceCents;
}
```

## Fix

Inline layers that only forward calls. Add them back when a second real implementation exists.

Source: [Martin Fowler: BeckDesignRules](https://martinfowler.com/bliki/BeckDesignRules.html)

Evidence: convention (widely taught by named practitioners, no controlled test).
