# Rule 32: Test behaviour through the public interface

## What it is

A test should check what the code does for its caller, not how it does it inside. Call the public function or endpoint with inputs, check the outputs or the visible effects. Don't test private methods, don't assert on the exact sequence of internal calls, and mock only what you don't control or can't run (payment gateways, SMS, the clock).

Shape of a good test:

- The name states the behaviour: `charges delivery for orders under 10 KWD`.
- Arrange, act, assert, in that order, with a blank line between.
- One behaviour per test. Several asserts are fine if they check that one behaviour.
- Plain literal values in the test. No logic (loops, conditionals) that could itself be wrong.

## Why it works

Google's review guide asks reviewers to check "the tests actually fail when the code is broken" and reminds them that "tests are also code that has to be maintained". Tests tied to internals break on every refactor even when behaviour is unchanged, so people stop trusting them. Heavy mocking produces tests that pass while the real integration is broken. Both Ousterhout and Martin are "huge fans" of unit testing; they only disagree on TDD as a process.

## Check

Rename a private method or reorder internal steps without changing behaviour. Do tests break? They shouldn't. Break the behaviour on purpose. Does a test fail? It should.

## Bad

```ts
it('works', () => {
  const spy = vi.spyOn(service, '_computeSubtotal');
  service.total(cart);
  expect(spy).toHaveBeenCalledTimes(1);
});
```

## Good

```ts
it('adds delivery fee to orders under the free delivery threshold', () => {
  const cart = [{ unitPriceFils: 2_500, quantity: 2 }];

  const total = calculateTotal(cart, { area: 'salmiya' });

  expect(total).toBe(5_000 + DELIVERY_FEE_FILS);
});
```

## Fix

Rewrite tests that spy on internals to assert on outputs. Replace mocks of your own code with the real thing. Use a real test database (SQLite, Testcontainers) for repository code instead of mocking the ORM.

Sources: [Google code review guide](https://google.github.io/eng-practices/review/reviewer/looking-for.html). Vladimir Khorikov, *Unit Testing* (Manning, 2020). [Kent C. Dodds: Testing implementation details](https://kentcdodds.com/blog/testing-implementation-details)

Evidence: convention (official guidance plus practitioner books).
