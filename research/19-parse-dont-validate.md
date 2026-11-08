# Rule 19: Parse input once at the boundary, then trust the types

## What it is

Data from outside (HTTP requests, forms, env vars, files, third-party APIs, the database) is untrusted. Convert it into a typed, checked value at the point it enters the system, and reject it there if it's wrong. Inside the system, functions accept the checked type and never re-check.

Alexis King's phrase for this is "parse, don't validate": a validator returns `true`/`false` and throws the knowledge away; a parser returns a new type that proves the check happened.

Go one step further where you can and make invalid states impossible to represent: use a union of states instead of a bag of optional fields.

## Why it works

Without this, the same checks get scattered across every function ("is this null? is the email valid?"), some get forgotten, and bugs appear far from where bad data came in. With it, the compiler enforces the rule: you can't pass a raw string where an `Email` is required.

## Check

Where does external data enter? Is there one schema check there (Zod, Valibot, FluentValidation, Laravel Form Requests, Pydantic)? Do inner functions accept `any`, `unknown` or raw `string`/`object` for values that should already be checked?

## Bad

```ts
type Payment = { status: string; paidAt?: Date; failureReason?: string; refundedAt?: Date };
// Any combination is possible, including status: 'paid' with failureReason set.

function sendReceipt(email: string) {
  if (!email.includes('@')) throw new Error('invalid'); // checked again here, and in 6 other places
}
```

## Good

```ts
const SignupSchema = z.object({ email: z.string().email(), age: z.number().int().min(18) });
type Signup = z.infer<typeof SignupSchema>;

export async function handleSignup(request: Request) {
  const signup = SignupSchema.parse(await request.json()); // the only check
  await createAccount(signup); // takes Signup, never re-checks
}

type Payment =
  | { status: 'pending' }
  | { status: 'paid'; paidAt: Date }
  | { status: 'failed'; failureReason: string }
  | { status: 'refunded'; paidAt: Date; refundedAt: Date };
```

## Fix

Add one schema at each entry point. Change inner functions to accept the parsed type. Replace optional-field bags with discriminated unions (TS), sealed records (C#) or enums with data (Rust, Swift).

Sources: [Alexis King: Parse, don't validate (2019)](https://lexi-lambda.github.io/blog/2019/11/05/parse-don-t-validate/). Yaron Minsky, "make illegal states unrepresentable" (Jane Street, *Effective ML*).

Evidence: convention (strongly held across typed-language communities; no controlled test).
