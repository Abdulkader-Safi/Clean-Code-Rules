# Rule 36: Pre-commit checklist

Run this on every change before calling it done. Each line links to the rule behind it.

## Fit

- [ ] Matches the codebase's existing conventions, not personal or model defaults ([01](01-follow-the-codebase-first.md))
- [ ] Reused what already exists (rule 31's ladder); no near-duplicate added ([31](31-reuse-before-writing.md))
- [ ] Nothing built for a future that wasn't asked for ([04](04-build-only-what-is-needed.md))
- [ ] Diff touches only what the task needs; tidying is in its own commit ([33](33-separate-tidying-from-behaviour.md))

## Names

- [ ] Full words, domain vocabulary, one word per concept ([05](05-names-reveal-intent.md), [06](06-one-word-per-concept.md))
- [ ] Booleans read as positive questions ([07](07-booleans-read-as-questions.md))
- [ ] Verbs match behaviour (`get` vs `fetch` vs `find`) ([08](08-verbs-for-functions-nouns-for-things.md))
- [ ] No magic numbers; units in names ([09](09-units-and-magic-numbers.md))

## Functions and flow

- [ ] Each function has a clear one-sentence purpose; no pass-through helpers ([10](10-hard-to-name-means-bad-design.md), [11](11-size-functions-by-depth.md))
- [ ] Guard clauses up top, main path flat ([12](12-guard-clauses-early-return.md))
- [ ] No bare boolean flags; 4+ params become an object ([13](13-few-arguments-no-flag-arguments.md))
- [ ] Business logic is pure; I/O at the edges ([14](14-separate-logic-from-side-effects.md))
- [ ] Inputs not mutated ([16](16-dont-mutate-inputs.md))
- [ ] Complex conditions named; no nested ternaries ([17](17-keep-cognitive-complexity-low.md), [18](18-boring-over-clever.md))

## Data and errors

- [ ] External input parsed once at the boundary ([19](19-parse-dont-validate.md))
- [ ] IDs and money have their own types; money is never a float ([20](20-avoid-primitive-obsession.md))
- [ ] No `!`, `as any` or `@ts-ignore` added without a reason ([21](21-handle-null-deliberately.md), [34](34-let-tools-enforce-style.md))
- [ ] No swallowed errors; wrapped errors keep their cause ([22](22-never-swallow-errors.md))

## Structure and docs

- [ ] New code sits with its feature ([26](26-group-by-feature.md))
- [ ] No new interface or layer with only one implementation ([25](25-deep-modules-hide-information.md), [27](27-dependencies-point-inward.md))
- [ ] Comments say why; exported functions have interface docs; no dead code ([24](24-comments-explain-why.md))

## Proof

- [ ] Tests cover the behaviour and fail if it breaks ([32](32-test-behaviour-not-implementation.md))
- [ ] Every project tool passes: formatter, linter, type checker, tests ([34](34-let-tools-enforce-style.md))
- [ ] Went through the AI failure list ([35](35-ai-failure-modes.md))
