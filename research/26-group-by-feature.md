# Rule 26: Group files by feature, not by technical type

## What it is

Put the code for one feature together: its route, handler, validation, queries, types, UI component and tests in one folder. Avoid top-level folders split only by kind (`controllers/`, `services/`, `models/`, `utils/`), where one feature is spread across six directories.

Jimmy Bogard calls this vertical slice architecture: minimise coupling between slices, maximise cohesion inside a slice. Shared code moves to a shared folder only once two or more features actually use it.

Follow the framework where it has strong conventions. Laravel expects `app/Models`, `app/Http/Controllers`; SvelteKit routes by folder. Inside those, group by feature where the framework allows (`app/Domain/Billing/`, `src/lib/features/billing/`).

## Why it works

Most changes are to one feature. When a feature's files sit together, a change touches one folder and the reader sees everything relevant at once. Deleting a feature means deleting a folder. Layer-first layouts make every change cross many folders and hide which files belong together.

## Check

Pick a recent feature. How many top-level folders did its change touch? Is there a `utils/` or `helpers/` file with unrelated functions from different features?

## Bad

```
src/
  controllers/  invoiceController.ts  userController.ts
  services/     invoiceService.ts     userService.ts
  validators/   invoiceValidator.ts   userValidator.ts
  utils/        helpers.ts            (formatting, dates, invoice maths, auth)
```

## Good

```
src/
  features/
    invoices/  routes.ts  create-invoice.ts  invoice-schema.ts  invoice-total.ts  invoice-total.test.ts
    users/     routes.ts  register-user.ts   user-schema.ts
  shared/      money.ts  dates.ts   (used by 2+ features)
```

## Fix

For new features, start a feature folder. Leave existing files where they are; moving them is a separate refactor for Safi to approve (rule 33).

Sources: [Jimmy Bogard: Vertical Slice Architecture](https://www.jimmybogard.com/vertical-slice-architecture/). Robert C. Martin, "Screaming Architecture" (2011): the folder structure should tell you what the app does, not which framework it uses.

Evidence: convention.
