# Rule 17: Name complex conditions and keep control flow flat

## What it is

Long boolean expressions, deep nesting, nested ternaries and loops with `break` and `continue` mixed in all make code slow to read. Pull a complex condition into a named variable or function that says what it means. Replace long `if/else if` chains that map a value to a result with a lookup table.

Cognitive complexity (SonarSource) is a useful measure here. It adds a point for each break in linear flow (`if`, loop, `catch`, `&&`/`||` sequences) and extra points for nesting. SonarQube's default limit is 15 per function.

## Why it works

A 2020 meta-analysis (Muñoz Barón, Wyrich, Wagner) of about 24,000 understandability ratings across 427 code snippets found cognitive complexity correlates positively with comprehension time and with developers' own ratings of how hard code is to understand. A named condition also documents the business rule: `isEligibleForRefund` says why, the raw expression only says how.

## Check

Any condition with more than two `&&`/`||` operators, any ternary inside a ternary, any function your linter scores above 15.

## Bad

```ts
if (order.status === 'delivered' && Date.now() - order.deliveredAt < 14 * DAY_MS && !order.items.some(i => i.final) && user.refunds < 3) { ... }

const label = status === 'a' ? 'Active' : status === 'p' ? 'Pending' : status === 's' ? 'Suspended' : 'Unknown';
```

## Good

```ts
const withinReturnWindow = Date.now() - order.deliveredAt < RETURN_WINDOW_MS;
const hasFinalSaleItem = order.items.some(item => item.isFinalSale);
const isEligibleForRefund =
  order.status === 'delivered' && withinReturnWindow && !hasFinalSaleItem && user.refundCount < MAX_REFUNDS;

if (isEligibleForRefund) { ... }

const STATUS_LABELS: Record<Status, string> = { active: 'Active', pending: 'Pending', suspended: 'Suspended' };
const label = STATUS_LABELS[status] ?? 'Unknown';
```

## Fix

Introduce explaining variables, replace chains with maps, and flatten with guard clauses (rule 12). Turn on the cognitive complexity rule in the linter (`sonarjs/cognitive-complexity` for ESLint) so this is checked automatically.

Sources: [SonarSource: Cognitive Complexity](https://www.sonarsource.com/blog/cognitive-complexity-because-testability-understandability/). [Muñoz Barón et al., ESEM 2020](https://arxiv.org/abs/2007.12520). Martin Fowler, *Refactoring*, "Extract Variable" and "Decompose Conditional".

Evidence: strong (meta-analysis).
