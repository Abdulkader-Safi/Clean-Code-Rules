# Rule 04: Build only what the task needs now

## What it is

YAGNI: "you aren't gonna need it". Don't add options, hooks, generic parameters, plugin systems or config for a future that hasn't been asked for. Solve today's problem in the simplest way that keeps tomorrow's change easy.

## Why it works

Google's review guide tells reviewers to be "especially vigilant about over-engineering" and to push authors to "solve the problem they know needs to be solved now". Speculative code still has to be read and tested. Most of the time the guessed-at future never arrives, or arrives in a shape the guess didn't fit.

YAGNI is not an excuse to skip input validation, error handling, security or accessibility. Those are today's problem.

## Check

For each parameter or branch in the change: is there a caller that uses it today? If not, remove it.

## Bad

```ts
function formatPrice(
  cents: number,
  options: { currency?: string; locale?: string; showSymbol?: boolean; roundingMode?: 'up' | 'down' | 'half' } = {},
) { /* 40 lines handling every option */ }
// Only ever called as formatPrice(total)
```

## Good

```ts
function formatKwd(fils: number): string {
  return new Intl.NumberFormat('en-KW', { style: 'currency', currency: 'KWD' }).format(fils / 1000);
}
```

## Fix

Delete unused options and branches. If a second currency arrives later, add the parameter then, with a real caller to shape it.

Sources: [Google code review guide](https://google.github.io/eng-practices/review/reviewer/looking-for.html). [Martin Fowler: Yagni](https://martinfowler.com/bliki/Yagni.html)

Evidence: strong (official guidance).
