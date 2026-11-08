# Rule 02: Write for the next reader

## What it is

Code is read many more times than it is written. The goal of every choice in this folder is the same: someone new (or Claude in a fresh session) should understand a piece of code quickly and change it without breaking something else.

Ousterhout defines complexity as anything about the structure of a system that makes it hard to understand and modify. It shows up three ways:

- Change amplification: a simple change needs edits in many places.
- Cognitive load: you have to hold a lot in your head to make a change.
- Unknown unknowns: it isn't obvious what you need to change, or what you'll break.

The third one is the worst.

## Why it works

Google's review guide defines "too complex" as "can't be understood quickly by code readers", and adds that it also means developers are likely to introduce bugs when they call or modify the code. Palomba et al. (30 projects, 17,350 validated smells) found classes with code smells are more change-prone and more fault-prone than clean ones.

## Check

Read the change as if you'd never seen the project. For each function ask: could I explain what it does in one sentence, without reading its body? Could I change it without opening three other files?

## Bad

```ts
const r = d.filter(x => x.s === 2 && !x.a).map(x => x.p * (1 - (x.dc ?? 0)));
```

## Good

```ts
const unpaidOrders = orders.filter(order => order.status === OrderStatus.Unpaid && !order.archived);
const amountsDue = unpaidOrders.map(order => order.price * (1 - (order.discountRate ?? 0)));
```

## Fix

When a piece of code took you more than a minute to understand, rename, restructure or comment it before moving on.

Sources: John Ousterhout, *A Philosophy of Software Design* (2nd ed., 2021), ch. 2. [Google code review guide](https://google.github.io/eng-practices/review/reviewer/looking-for.html). [Palomba et al., 2018, Empirical Software Engineering](https://research.tudelft.nl/en/publications/on-the-diffuseness-and-the-impact-on-maintainability-of-code-smel)

Evidence: strong (large empirical study plus official guidance).
