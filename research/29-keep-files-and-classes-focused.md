# Rule 29: One reason to change per file or class

## What it is

The single responsibility principle, stated the way Martin later clarified it: a module should be responsible to one actor, meaning one group of people who would ask for it to change. An `Invoice` class that also formats PDFs for the finance team and sends emails for marketing has three reasons to change.

Soft limits that prompt a look (not rules):

- A file over about 300 to 400 lines
- A class with more than about 10 public methods
- A component that fetches data, holds lots of state and renders a complex UI
- A file name ending in `Manager`, `Helper`, `Utils` or `Service` that keeps growing

## Why it works

Large classes with many responsibilities ("God classes", "Complex Class") are the most common smells in Palomba et al.'s study of 30 projects, and smelly classes were more change-prone and fault-prone. When unrelated responsibilities share a file, a change for one actor risks breaking the other, and merge conflicts become constant.

## Check

Can you describe what the file or class is for in one sentence without "and"? Who would ask for it to change: one team, or several?

## Bad

A 1,200-line `OrderService.ts` with pricing, PDF generation, email templates, warehouse sync and admin reporting.

## Good

```
features/orders/
  order-pricing.ts      // finance rules
  order-invoice-pdf.ts  // document layout
  order-emails.ts       // customer messages
  warehouse-sync.ts     // integration
```

## Fix

Split along the actors. Don't split a cohesive 500-line file just to get under a number; a long file that does one thing well is fine (rule 11 applies to files too).

Sources: [Robert C. Martin: The Single Responsibility Principle (2014)](https://blog.cleancoder.com/uncle-bob/2014/05/08/SingleReponsibilityPrinciple.html). [Palomba et al., 2018](https://research.tudelft.nl/en/publications/on-the-diffuseness-and-the-impact-on-maintainability-of-code-smel)

Evidence: strong for "large complex classes are more fault-prone"; convention for the exact limits.
