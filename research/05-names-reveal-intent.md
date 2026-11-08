# Rule 05: Names say what a thing is, in full words

## What it is

A name should tell the reader what the value holds or what the function does, without them reading the body. Use full words, not abbreviations. Make the length fit the scope: a loop index over three lines can be `i`; a module-level export needs a full descriptive name.

Allowed short forms: well-known ones everyone in the field reads instantly (`id`, `url`, `html`, `db`, `i` and `j` in tight loops, `e` or `err` in a catch block). Everything else gets spelled out.

## Why it works

Hofmeister et al. (2017) had 72 professional C# developers find bugs in code snippets. With full-word identifiers they were about 19% faster than with single letters or abbreviations. Two other 2017 studies found no significant difference, so the effect isn't settled, but no study found abbreviations helped. Google's guide: "A good name is long enough to fully communicate what the item is or does, without being so long that it becomes hard to read."

## Check

Read each new name out of context. Could a new teammate guess what it holds? Would they need the type, the body or a comment to know?

## Bad

```ts
const d = getData();
const usrLst = d.filter(u => u.act);
let tmp = 0;
function proc(o) { ... }
```

## Good

```ts
const response = await fetchCustomers();
const activeCustomers = response.customers.filter(customer => customer.isActive);
let overdueCount = 0;
function sendInvoiceReminder(invoice: Invoice) { ... }
```

## Fix

Rename in one go with the editor's rename tool so every reference moves together. Avoid filler words that add length without meaning: `data`, `info`, `item`, `object`, `value`, `manager`, `helper`, `util`, `handle`, `process`, `temp`.

Sources: [Hofmeister, Siegmund, Holt: Shorter identifier names take longer to comprehend (SANER 2017)](https://www.se.cs.uni-saarland.de/publications/docs/HoSeHo17.pdf). [Never Work in Theory: abbreviated vs full-word names](https://neverworkintheory.org/2021/08/09/abbreviated-vs-full-names.html). [Google code review guide](https://google.github.io/eng-practices/review/reviewer/looking-for.html)

Evidence: strong but mixed (one controlled study for, two with no difference, none against).
