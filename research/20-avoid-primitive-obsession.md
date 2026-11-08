# Rule 20: Give important values their own types

## What it is

"Primitive obsession" is using plain strings and numbers for things that have rules: IDs, money, email addresses, phone numbers, durations, percentages. Give them small types of their own so they can't be mixed up and their rules live in one place.

Money is the important case: never store or calculate money in floating point. Use integer minor units (fils, cents) or a decimal type.

## Why it works

`chargeCustomer(orderId, customerId)` compiles even when the two are swapped, because both are `string`. A branded type makes it a compile error. Money is worse: `0.1 + 0.2` is `0.30000000000000004` in JavaScript, and after enough line items and rounding steps that's how an invoice ends up one fils off and nobody can find out why.

## Check

Are there two parameters of the same primitive type next to each other? Is money ever a `number`/`float`/`double`? Is a phone number or civil ID validated in more than one place?

## Bad

```ts
function refund(orderId: string, userId: string, amount: number) { ... }
refund(user.id, order.id, 12.5); // swapped, compiles, wrong
```

## Good

```ts
type OrderId = string & { readonly __brand: 'OrderId' };
type UserId = string & { readonly __brand: 'UserId' };
type Fils = number & { readonly __brand: 'Fils' }; // integer, 1 KWD = 1000 fils

function refund(orderId: OrderId, userId: UserId, amount: Fils) { ... }
```

```csharp
public readonly record struct OrderId(Guid Value);
public readonly record struct Money(long MinorUnits, string Currency);
// use decimal, never double, if minor units don't fit
```

```php
// Laravel: cast to a value object
protected function casts(): array { return ['price' => MoneyCast::class]; }
```

## Fix

Start with IDs and money, where mix-ups cost the most. Don't wrap every string; a `title` or `description` is fine as a string.

Sources: Martin Fowler, *Refactoring*, smell "Primitive Obsession" and "Replace Primitive with Object". [Martin Fowler: Money pattern](https://martinfowler.com/eaaCatalog/money.html)

Evidence: convention (the floating point money problem is a documented fact of IEEE 754).
