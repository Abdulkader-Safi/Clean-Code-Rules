# Rule 18: Boring code over clever code

## What it is

Prefer the obvious way to write something over the shortest or most impressive way. No one-liners that need a comment to decode, no operator tricks, no metaprogramming where a plain function works, no custom DSL where a list of objects would do.

## Why it works

Clever code is cheap to write and expensive to read, and it's read at the worst time: during a bug at 3am. Ousterhout's red flag "nonobvious code" covers it: if the meaning can't be grasped with a quick read, it costs every future reader. Brian Kernighan's line applies: debugging is twice as hard as writing the code, so if you write it as cleverly as you can, you're by definition not smart enough to debug it.

## Check

Would a mid-level developer new to this language understand the line on first read? Does it rely on operator precedence, implicit type coercion, or a language feature most of the team hasn't used?

## Bad

```ts
const flags = ~~(n / 2) | (+!!active << 1);
const unique = [...new Map(rows.map(r => [r.id, r])).values()].reduce((a, r) => (a[r.type] ??= []).push(r) && a, {});
```

## Good

```ts
const latestRowById = new Map(rows.map(row => [row.id, row]));
const rowsByType = Object.groupBy(latestRowById.values(), row => row.type);
```

## Fix

Rewrite in steps with named intermediate values. If a clever version is needed for performance, keep it, measure it, and add a comment saying why and what it replaced.

Sources: John Ousterhout, *A Philosophy of Software Design*, ch. 18 "Code should be obvious". Brian Kernighan and P. J. Plauger, *The Elements of Programming Style* (1974). [The Grug Brained Developer](https://grugbrain.dev/)

Evidence: convention.
