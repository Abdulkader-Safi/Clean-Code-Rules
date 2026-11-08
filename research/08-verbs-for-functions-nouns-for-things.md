# Rule 08: Functions are verbs, types are nouns, and verbs keep their meaning

## What it is

Functions and methods do something, so they start with a verb: `calculateTax`, `sendReceipt`, `parseDate`. Classes, types and modules are things, so they're nouns: `Invoice`, `TaxRule`, `ReceiptMailer`.

Give common verbs one fixed meaning across the project:

| Verb | Means |
|---|---|
| `get` | Cheap, no I/O, from memory (a getter, a map lookup) |
| `fetch` / `load` | Goes over the network or to the database, may fail, usually async |
| `find` | May return nothing (`null` or empty) |
| `compute` / `calculate` | Pure work from inputs, no side effects |
| `create` / `build` | Returns a new thing |
| `save` / `update` / `delete` | Writes |
| `to` / `as` | Converts (`toJson`, `asReadonly`) |
| `is` / `has` / `can` | Returns a boolean (rule 07) |

Use whatever split the codebase already has (rule 01). The point is that a verb means one thing.

## Why it works

If `getUser()` sometimes hits the database, callers can't tell which calls are expensive or can fail. A name that promises less than the function does is how surprise N+1 queries and unhandled rejections happen.

Nouns like `Manager`, `Helper`, `Utils`, `Handler`, `Processor` or `Service` on their own say almost nothing about what's inside. They become dumping grounds.

## Check

For each function: does the verb match what the body does (I/O or not, can return nothing or not, writes or not)? For each class: could you say what it holds or does without the suffix?

## Bad

```ts
function getUser(id: string) { return db.users.findOne(id); } // I/O behind "get"
class UserManager { ... } // 40 unrelated methods
function data(): Order[] { ... }
```

## Good

```ts
async function fetchUser(id: UserId): Promise<User | null> { return db.users.findOne(id); }
class PasswordResetMailer { ... }
function listOpenOrders(): Order[] { ... }
```

## Fix

When a `*Manager` or `*Utils` file grows, split it by what each group of functions is about (rule 26).

Sources: Robert C. Martin, *Clean Code*, ch. 2. [Microsoft .NET naming guidelines](https://learn.microsoft.com/en-us/dotnet/standard/design-guidelines/naming-guidelines). [qntm: It's probably time to stop recommending Clean Code](https://qntm.org/clean) (points out even the book breaks its verb rule with `wayTooCold()`).

Evidence: convention.
