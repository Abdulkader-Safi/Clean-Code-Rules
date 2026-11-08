# Rule 10: If it's hard to name, the design is wrong

## What it is

When you can't find a short, precise name for a function or class, that's usually because it does two unrelated things, or half of one thing. Treat the struggle as a design signal, not a vocabulary problem. The same goes for comments: if you can't describe what a function does in one or two plain sentences, it probably needs restructuring.

## Why it works

Ousterhout lists both as red flags: "Hard to pick name" and "Hard to describe". A function named `validateAndSaveAndNotify` is honest about doing three jobs. A function named `handleSubmit` that secretly does the same three is worse, because the name hides it.

## Check

If the best name you can find contains "and", "or", "then", or a vague verb like `handle`, `process`, `manage`, `do`, stop and look at the body. Does it split into pieces that each have a clear name?

## Bad

```ts
async function processOrder(order: Order) {
  // validates, charges the card, writes to DB, emails the customer, updates analytics
}
```

## Good

```ts
async function placeOrder(order: Order) {
  const validOrder = validateOrder(order);
  const payment = await chargeCard(validOrder);
  const saved = await saveOrder(validOrder, payment);
  await sendOrderConfirmation(saved);
}
```

Here `placeOrder` is a fine name because it describes the whole flow, and each step has its own clear name.

## Fix

Split along the seams the naming problem revealed. If nothing splits cleanly, the name might be fine and just needs to be broader and honest (`placeOrder`, not `saveOrder`).

Source: John Ousterhout, *A Philosophy of Software Design*, red flags list. [Summary of the red flags](https://dev.to/sportebois/software-design-red-flags-wisdom-nuggets-from-john-ousterhout-43i2)

Evidence: convention.
