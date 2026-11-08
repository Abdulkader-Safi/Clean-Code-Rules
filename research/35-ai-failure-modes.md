# Rule 35: Watch for the mistakes AI coding agents make most

## What it is

Coding agents, Claude included, have recognisable bad habits. This list is what the skill should check hardest, because these are the mistakes a human reviewer finds again and again in agent-written code.

1. Writing a new helper instead of finding the existing one (rule 31). GitClear's data on 211M lines shows copy-pasted blocks up eightfold in 2024 and refactored ("moved") code down 39.9%.
2. Over-engineering: interfaces with one implementation, factories, config options nobody asked for, generic type parameters for one type (rules 03, 04).
3. Defensive noise: `try/catch` at every level, null checks on values the types already guarantee, fallback values that hide bugs (rules 21, 22).
4. Comments that narrate the code: `// Loop through the users`, `// Return the result` (rule 24).
5. Changing unrelated code: reformatting, renaming, "improving" files the task didn't touch (rule 33).
6. Ignoring the project's conventions and writing in the model's default style instead (rule 01).
7. Leaving dead code behind: the old function next to the new one, unused imports, commented-out blocks.
8. Tests that test mocks: asserting a mock was called with what the mock was set up to return (rule 32).
9. Weakening checks to make an error go away: `as any`, `!`, `@ts-ignore`, deleting a failing test, catching and returning `null` (rules 21, 34).
10. Placeholder logic left in: `// TODO: implement`, hard-coded sample data, `return true` stubs, presented as finished.
11. Inventing APIs: calling a library method or config key that doesn't exist in the installed version.
12. Fixing the symptom at one call site instead of the root cause in the shared function that every caller uses.

## Why it works

Each of these makes a change look complete while adding hidden cost that someone else pays later, usually during the next bug hunt or the next feature that touches the same file. A named list helps. The agent can check its own diff against it line by line.

## Check

Before finishing, read the full diff and go through the 12 items above. For 11, check the installed version's docs or types. For 12, search for every caller of the function you changed.

## Bad

```ts
// Function to get user by ID
export async function getUserById(id: string): Promise<User | null> {
  try {
    if (!id) return null;
    const user = await db.user.findUnique({ where: { id } });
    return user ?? null;
  } catch (error) {
    console.error('Error fetching user:', error);
    return null;
  }
}
// existing src/lib/users.ts already exports findUser(id)
```

## Good

```ts
import { findUser } from '@/lib/users';
```

## Fix

Run this check on every change. Delete what it catches.

Sources: [DevClass on GitClear's 2025 report](https://devclass.com/2025/02/20/ai-is-eroding-code-quality-states-new-in-depth-report/). [LeadDev: How AI generated code accelerates technical debt](https://leaddev.com/software-quality/how-ai-generated-code-accelerates-technical-debt). Items 2 to 12 are common review findings, not from a study.

Evidence: vendor for item 1; anecdote for the rest.
