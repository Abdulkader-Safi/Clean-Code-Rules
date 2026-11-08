# Rule 15: Keep one level of detail per function

## What it is

A function that describes a high-level flow ("validate, charge, save, notify") shouldn't also contain low-level detail (string slicing, SQL, byte maths) in the middle of it. Mixing levels makes the reader switch between "what is happening" and "how exactly" on every line.

This doesn't mean extract everything (see rule 11). It means: when a function reads as a list of steps, each step should be at roughly the same height.

## Why it works

When high-level steps are mixed with detail, the outline of the function disappears. Readers skim for the shape first, then dive into the step they care about. Keeping levels apart lets them do that.

## Check

Read the function top to bottom. Does it jump from a business step to a regex to a business step? The low-level lines are candidates for a named helper.

## Bad

```ts
async function registerUser(input: SignupForm) {
  const email = input.email.trim().toLowerCase();
  if (!/^[^@\s]+@[^@\s]+\.[^@\s]+$/.test(email)) throw new Error('bad email');
  const user = await createUser(email, input.password);
  const token = crypto.randomBytes(32).toString('hex');
  await db.query('INSERT INTO verify_tokens (user_id, token, expires) VALUES ($1, $2, now() + interval \'1 day\')', [user.id, token]);
  await sendWelcomeEmail(user);
}
```

## Good

```ts
async function registerUser(input: SignupForm) {
  const email = parseEmail(input.email);
  const user = await createUser(email, input.password);
  const token = await createVerificationToken(user.id);
  await sendWelcomeEmail(user, token);
}
```

## Fix

Pull the low-level lines into helpers named after what they achieve, not how.

Source: Robert C. Martin, *Clean Code*, ch. 3 "One level of abstraction per function".

Evidence: convention.
