# Rule 22: Fail loudly, catch only where you can act, keep the cause

## What it is

- Catch an error only where you can do something useful with it: retry, fall back, turn it into a user-facing message, or add context and rethrow.
- Never catch and ignore. Never catch, log and carry on as if nothing happened, unless carrying on is genuinely correct and the comment says why.
- When you wrap an error, keep the original as the cause.
- Error messages say what failed, with which input, and what to do about it if that's known.
- Have one top-level handler per entry point (HTTP middleware, job runner, CLI main) that logs and returns a clean response. Most code in between needs no `try` at all.

## Why it works

A swallowed error turns a clear crash into silent wrong data, discovered weeks later. Wrapping in `try/catch` "just in case" at every level adds noise and usually hides the stack trace that would have pointed to the bug. Ousterhout makes the related point that exception handling is one of the worst sources of complexity, because handlers are rarely tested.

## Check

Search the change for `catch`. For each one: what does it do with the error? Empty, `console.log` only, or `return null` without a comment are all red flags. Is the original error passed as `cause`?

## Bad

```ts
try {
  await chargeCard(order);
} catch (e) {
  console.log(e); // order continues as if paid
}

throw new Error('Something went wrong');
```

## Good

```ts
try {
  await chargeCard(order);
} catch (error) {
  throw new PaymentFailedError(`Charging order ${order.id} failed`, { cause: error });
}

throw new Error(`Invoice ${invoiceId} has no line items; add at least one before sending`);
```

```php
// Laravel: let it bubble to the exception handler, report with context there
throw new PaymentFailed("Charging order {$order->id} failed", previous: $e);
```

## Fix

Delete catches that don't act. Add `cause`. Move logging to the top-level handler. Never put secrets, tokens or full card numbers in error messages or logs.

Sources: John Ousterhout, *A Philosophy of Software Design*, ch. 10 "Define errors out of existence". [MDN: Error cause](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Error/cause). [Google code review guide](https://google.github.io/eng-practices/review/reviewer/looking-for.html)

Evidence: convention.
