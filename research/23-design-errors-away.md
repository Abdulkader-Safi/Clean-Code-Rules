# Rule 23: Design errors out of existence where you can

## What it is

Before adding an error case, ask whether the operation can be defined so the error can't happen. Ousterhout's examples: deleting a file that doesn't exist can count as success (the goal, "file is gone", is met). Taking a substring past the end of a string can return what's there instead of throwing.

Common cases in web work:

- Idempotent operations: "cancel subscription" on an already-cancelled one succeeds and does nothing.
- Upserts instead of "insert, catch duplicate, then update".
- Defaults that make sense instead of throwing on a missing optional setting.
- Database constraints (unique, foreign key, not null) instead of app code that checks first and races.

## Why it works

Every error a function can throw is work for every caller. Fewer error cases means less handling code, fewer untested paths, and fewer bugs in the handlers themselves. Retries (network, queues, double-clicks) become safe when operations are idempotent.

This doesn't mean hiding real failures. A failed payment is a real error. A second click on "cancel" is not.

## Check

For each new error the change can throw: is it a real failure the caller must know about, or a case the function could handle by definition?

## Bad

```ts
async function cancelSubscription(id: SubscriptionId) {
  const sub = await findSubscription(id);
  if (sub.status === 'cancelled') throw new Error('Already cancelled');
  ...
}
```

## Good

```ts
async function cancelSubscription(id: SubscriptionId): Promise<void> {
  const sub = await fetchSubscription(id);
  if (sub.status === 'cancelled') return; // goal already met
  ...
}
```

```sql
INSERT INTO page_views (page_id, day, count) VALUES ($1, $2, 1)
ON CONFLICT (page_id, day) DO UPDATE SET count = page_views.count + 1;
```

## Fix

Make write operations idempotent. Push uniqueness rules into the database. Turn "already done" errors into early returns.

Source: John Ousterhout, *A Philosophy of Software Design*, ch. 10 "Define errors out of existence".

Evidence: convention.
