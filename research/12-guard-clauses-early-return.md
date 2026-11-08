# Rule 12: Handle the edge cases first and return early

## What it is

Check for invalid input, missing data and edge cases at the top of a function and return (or throw) straight away. The main logic then runs at the lowest indent, without being wrapped in `if` blocks. These checks are called guard clauses.

## Why it works

Each level of nesting is one more condition the reader has to remember. SonarSource's cognitive complexity metric adds extra cost for every level of nesting for this reason, and a 2020 meta-analysis of about 24,000 understandability ratings across 427 snippets found the metric correlates with how long developers take to understand code.

## Check

Look for functions where the main work sits two or more levels deep, or for an `else` after a block that ends in `return`.

## Bad

```ts
function applyDiscount(order: Order | null, code: string) {
  if (order) {
    if (order.status === 'open') {
      const discount = findDiscount(code);
      if (discount) {
        if (!discount.expired) {
          order.total -= discount.amount;
          return order;
        } else {
          throw new Error('Discount expired');
        }
      }
    }
  }
  return order;
}
```

## Good

```ts
function applyDiscount(order: Order, code: string): Order {
  if (order.status !== 'open') return order;

  const discount = findDiscount(code);
  if (!discount) return order;
  if (discount.expired) throw new DiscountExpiredError(code);

  return { ...order, total: order.total - discount.amount };
}
```

## Fix

Invert each condition, return early, and delete the `else`. Make `null` impossible at the type level where you can, so the guard isn't needed (rule 19).

Sources: Martin Fowler, *Refactoring* (2nd ed.), "Replace Nested Conditional with Guard Clauses". [SonarSource: Cognitive Complexity](https://www.sonarsource.com/blog/cognitive-complexity-because-testability-understandability/). [Muñoz Barón, Wyrich, Wagner: An Empirical Validation of Cognitive Complexity (ESEM 2020)](https://arxiv.org/abs/2007.12520)

Evidence: strong (meta-analysis supports the metric that penalises nesting).
