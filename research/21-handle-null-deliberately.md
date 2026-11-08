# Rule 21: Decide where "nothing" is allowed, and say so in the type

## What it is

Turn on strict null checks (`strict: true` in TypeScript, nullable reference types in C#, `declare(strict_types=1)` and typed properties in PHP). Then:

- A function that may find nothing says so in its return type (`User | null`, `User?`, `?User`) and is named for it (`findUser`, not `getUser`).
- Functions that return lists return an empty list, never `null`.
- Use `null` or `undefined` for "absent", never magic values like `-1`, `""` or `0`.
- Don't scatter `?.` and `!` to make the compiler stop complaining. Handle absence once, near where it starts.

## Why it works

Tony Hoare called the null reference his "billion-dollar mistake". With strict null checks, the compiler tells you every place that has to handle "nothing". Without them, it shows up in production as `Cannot read properties of undefined`. Returning an empty array instead of `null` removes a whole class of checks from every caller.

## Check

Is strict null checking on? Any `!` (non-null assertion) or `as` cast in the change? Any collection-returning function with `null` in its type? Any `?.` chain more than two deep?

## Bad

```ts
function getOrders(userId: string): Order[] | null { ... }
const count = getOrders(id)!.length;
const city = user?.profile?.address?.city?.name ?? '';
function indexOfProduct(sku: string): number { return -1; } // -1 means not found
```

## Good

```ts
function listOrders(userId: UserId): Order[] { ... } // [] when none
function findProduct(sku: Sku): Product | null { ... }

const product = findProduct(sku);
if (!product) return notFound(sku);
// product is Product from here on
```

## Fix

Enable strict mode in the project config if it's off (raise this with Safi first on an existing project; it can surface hundreds of errors). Replace `!` with a real check or a type that can't be null.

Sources: Tony Hoare, "Null References: The Billion Dollar Mistake" (QCon London, 2009). [TypeScript handbook: strictNullChecks](https://www.typescriptlang.org/tsconfig/#strictNullChecks). [Microsoft: nullable reference types](https://learn.microsoft.com/en-us/dotnet/csharp/nullable-references)

Evidence: convention (official language guidance).
