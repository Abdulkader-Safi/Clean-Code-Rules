# Rule 16: Don't mutate what you were given; prefer immutable values

## What it is

A function shouldn't change the objects or arrays passed into it unless its name says it does (`sortInPlace`, `appendTo`). Return a new value instead. Default to `const`, `readonly`, `final`, records and immutable collections, and use mutable state only where it's local and contained.

No output parameters: if a function needs to give back two things, return an object or tuple.

## Why it works

Mutating an argument changes data the caller still holds, often far from the call. That's how a list gets sorted in one component and shows up re-ordered in another. Immutable values can be shared and cached safely, and they make the flow of data visible in the code.

## Check

Search the change for assignments to parameter fields, and for `push`, `splice`, `sort`, `reverse` or `delete` on something that came in as an argument. Look for `let` that never gets reassigned.

## Bad

```ts
function addTax(order: Order) {
  order.total = order.total * 1.05; // caller's object silently changed
  return order;
}
const sorted = users.sort(byName); // also sorts `users`
```

## Good

```ts
function withTax(order: Order): Order {
  return { ...order, total: order.total * TAX_MULTIPLIER };
}
const sorted = users.toSorted(byName); // ES2023; or [...users].sort(byName)
```

```csharp
public record Order(Guid Id, decimal Total);
var taxed = order with { Total = order.Total * TaxMultiplier };
```

## Fix

Return a copy. Mark fields `readonly` and types `Readonly<T>` so the compiler catches future mutations. Local mutation inside a function (building an array in a loop) is fine.

Sources: Robert C. Martin, *Clean Code*, ch. 3 "Have no side effects" and "Output arguments". Joshua Bloch, *Effective Java* (3rd ed.), item 17 "Minimize mutability".

Evidence: convention.
