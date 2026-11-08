# Rule 14: Keep calculations pure and push I/O to the edges

## What it is

Split code into two kinds:

- Pure functions (the "functional core"): take inputs, return outputs, touch nothing else. No database, no network, no clock, no randomness, no global state.
- Thin edge code (the "imperative shell"): reads the request, loads data, calls the pure functions, writes results, sends the response.

Business rules (pricing, eligibility, scheduling, permissions) belong in the core. Gary Bernhardt named this "functional core, imperative shell".

A related rule, command-query separation: a function either answers a question (returns data, changes nothing) or does something (changes state). Not both.

## Why it works

Pure functions are trivial to test (no mocks, no setup), safe to call twice, and easy to reason about because everything they depend on is in the signature. qntm's main complaint about *Clean Code*'s examples is code that works "almost entirely by setting and reading member variables", which makes it "effectively untestable".

## Check

Can the main business logic in the change be tested with plain values in and plain values out? Does any function that sounds like a question (`isValid`, `getTotal`) also write something?

## Bad

```ts
async function checkout(cartId: string) {
  const cart = await db.carts.find(cartId);
  let total = 0;
  for (const item of cart.items) {
    const product = await db.products.find(item.productId);
    total += product.price * item.qty;
    if (new Date().getDay() === 5) total *= 0.9; // Friday sale buried in a loop
  }
  await db.orders.insert({ cartId, total });
  return total;
}
```

## Good

```ts
// Core: pure, testable with plain objects
export function calculateTotal(items: CartLine[], today: Date): number {
  const subtotal = items.reduce((sum, line) => sum + line.unitPrice * line.quantity, 0);
  return isFridaySale(today) ? subtotal * FRIDAY_DISCOUNT : subtotal;
}

// Shell: loads, calls the core, saves
export async function checkout(cartId: CartId) {
  const lines = await loadCartLines(cartId);
  const total = calculateTotal(lines, new Date());
  await db.orders.insert({ cartId, total });
  return total;
}
```

## Fix

Pull the decision-making out of the I/O function into a pure one. Pass the clock and config in as arguments instead of reading them inside.

Sources: [Gary Bernhardt: Functional Core, Imperative Shell](https://www.destroyallsoftware.com/screencasts/catalog/functional-core-imperative-shell). [Martin Fowler: CommandQuerySeparation](https://martinfowler.com/bliki/CommandQuerySeparation.html). [qntm review](https://qntm.org/clean)

Evidence: convention.
