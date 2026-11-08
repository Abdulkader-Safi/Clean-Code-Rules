# Rule 25: Modules should be deep: simple interface, real work behind it

## What it is

A module (function, class, file, package, service) is deep when its interface is small and simple compared to what it does. Unix file I/O is the classic example: five calls (`open`, `read`, `write`, `lseek`, `close`) hide disk layout, caching, permissions and devices.

Each module should hide a design decision that callers don't need to know: a file format, which database, a retry policy, an API's quirks. If that decision changes, only one module changes.

Red flags from Ousterhout's list:

- Shallow module: the interface is nearly as complicated as the code behind it.
- Pass-through method: it only forwards its arguments to another method with a similar signature.
- Information leakage: the same knowledge (a format, a field order, a magic string) appears in two modules, so both change together.
- Temporal decomposition: code split by when things happen (read, then parse, then write) instead of by what knowledge they share.
- Overexposure: callers have to learn rare options to use the common case.

## Why it works

Every interface is something callers must learn. A shallow class adds an interface without removing any complexity, so it makes the system harder overall. Information leakage is how one change turns into edits across six files (change amplification).

## Check

For each new class or module: is the interface smaller than the implementation? Could a caller use it without knowing how it works inside? Does the same knowledge appear in two places?

## Bad

```ts
class UserRepository {
  findById(id: string) { return this.db.users.findById(id); }
  save(user: User) { return this.db.users.save(user); }
  delete(id: string) { return this.db.users.delete(id); }
} // pure pass-through over an ORM that already does this
```

## Good

```ts
// Hides the provider, auth, retries, rate limits and response format
export async function sendSms(to: PhoneNumber, message: string): Promise<void> { ... }
```

## Fix

Inline pass-through layers. Merge modules that always change together. Move shared knowledge into one module that owns it. Give the common case a simple call with sensible defaults.

Sources: John Ousterhout, *A Philosophy of Software Design*, ch. 4 to 5 and the red flags list. David Parnas, "On the Criteria To Be Used in Decomposing Systems into Modules" (1972).

Evidence: convention (Parnas 1972 is the foundation; widely taught).
