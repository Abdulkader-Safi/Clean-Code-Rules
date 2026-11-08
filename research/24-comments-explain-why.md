# Rule 24: Comments explain why; code explains what

## What it is

Write a comment when it tells the reader something the code can't: why this approach, why not the obvious one, what business rule this encodes, what will break if you change it, a link to the ticket or the spec. Don't write comments that repeat the code.

Two kinds of comment are worth writing:

- Interface comments on exported functions, classes and modules: what it does, what the inputs mean, what it returns, what it throws, any side effects. Callers should never need to read the body.
- "Why" comments inside a body, next to anything surprising.

Delete commented-out code (it's in git). Every `TODO` gets a reason or a ticket.

## Why it works

This is where Martin and Ousterhout disagree most. Martin treats "every comment as potential misinformation" and prefers long names. Ousterhout says "the cost of missing comments is easily 10-100x the cost of incorrect comments". Google's guide sides with the middle: comments "are useful when they explain why some code exists", and "if the code isn't clear enough to explain itself, then the code should be made simpler." Both authors agreed in their debate that some comments are necessary and that comments repeating the code are useless.

## Check

For each comment: would deleting it lose information? If no, delete. For each exported function: could a caller use it correctly from the signature and doc comment alone? For each surprising line: is there a comment saying why?

## Bad

```ts
// Increment i
i++;

// Get the user
const user = await fetchUser(id);

// const oldTotal = calc(items) * 1.1;

// TODO fix this
```

## Good

```ts
/**
 * Returns the delivery fee in fils for a Kuwait address.
 * Free above FREE_DELIVERY_THRESHOLD_FILS. Throws UnknownAreaError if the area isn't in the zone table.
 */
export function deliveryFeeFils(area: AreaCode, subtotalFils: Fils): Fils { ... }

// Talabat's webhook retries up to 5 times and doesn't send an idempotency key,
// so dedupe on (orderId, eventType) before processing.
if (await alreadyProcessed(event.orderId, event.type)) return;

// TODO(#412): switch to the bulk endpoint once the Hesabate API supports it.
```

## Fix

Delete what-comments and dead code. Add interface docs to anything exported. When you write a hack or a workaround, write the reason next to it.

Sources: [Ousterhout and Martin debate](https://github.com/johnousterhout/aposd-vs-clean-code). [Google code review guide](https://google.github.io/eng-practices/review/reviewer/looking-for.html). John Ousterhout, *A Philosophy of Software Design*, ch. 12 to 13.

Evidence: contested on how many comments; agreed on why-not-what.
