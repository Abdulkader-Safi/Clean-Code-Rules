# Rule 27: Business logic doesn't import the framework; inject at the edges

## What it is

Core rules (pricing, permissions, scheduling) shouldn't import the HTTP framework, the ORM or a vendor SDK directly. The edges (controllers, jobs, CLI commands) do the I/O and pass plain data in. When the core needs something external (send an email, read the time), pass it in as a function or object argument instead of reaching for a global or a singleton.

Keep this proportionate. An interface with one implementation, created only "for testability", is a shallow module (rule 25). Add an interface when there is a real boundary: a third-party service you might swap, or something you can't run in tests (payments, SMS).

## Why it works

Logic that doesn't depend on the framework can be tested with plain values and survives framework upgrades. Hidden dependencies (a global `db`, `Date.now()` deep inside, a static `Mail::send`) make code hard to test and hard to reason about, because the signature lies about what the function needs.

## Check

Does the core logic file import the framework's request/response types, the ORM, or an SDK? Does a function read the clock, env vars or a global inside its body when it could take them as arguments?

## Bad

```ts
import { db } from '@/lib/db';
import { stripe } from '@/lib/stripe';

export async function renewSubscription(userId: string) {
  const user = await db.user.findUnique({ where: { id: userId } });
  if (new Date() > user.renewsAt) await stripe.charges.create({ ... });
}
```

## Good

```ts
// core: pure decision
export function isDueForRenewal(subscription: Subscription, now: Date): boolean {
  return now >= subscription.renewsAt && subscription.status === 'active';
}

// edge: does the I/O
export async function renewDueSubscriptions(deps: { db: Db; payments: PaymentGateway; now: Date }) {
  const subscriptions = await deps.db.activeSubscriptions();
  for (const subscription of subscriptions.filter(s => isDueForRenewal(s, deps.now))) {
    await deps.payments.charge(subscription.customerId, subscription.priceFils);
  }
}
```

## Fix

Pull decisions into pure functions (rule 14). Pass dependencies as arguments. Use the framework's container (Laravel, ASP.NET Core) at the edges only.

Sources: Robert C. Martin, *Clean Architecture* (2017), "The Dependency Rule". [Gary Bernhardt: Functional Core, Imperative Shell](https://www.destroyallsoftware.com/screencasts/catalog/functional-core-imperative-shell). Mark Seemann, *Dependency Injection Principles, Practices, and Patterns* (2019).

Evidence: convention.
