# Clean code: rules for writing code Claude should follow on every project

36 rules and one reference file, one per file. They cover how to name things, write functions, structure a codebase and make a change. Each file has what the rule is, why it works, a check, a bad example, a good example, the fix and its sources. Examples are mostly TypeScript, with C#, PHP and SQL where the rule differs.

Researched 2026-11-07 as the source material for a Claude skill. The skill itself isn't built yet.

## The core finding

The classic advice (Robert C. Martin's *Clean Code*, 2008) has been heavily criticised since, and the critics have a point on two things: tiny functions and few comments. John Ousterhout's *A Philosophy of Software Design* is now the more trusted reference for design. In 2025 the two authors debated publicly and agreed on most things, which is useful: where they agree, the rule is solid.

What every source agrees on:

- Code is for the next reader. "Too complex" means "can't be understood quickly" (Google).
- Names matter more than almost anything else, and should say what a thing is in full words.
- Hide complexity behind simple interfaces. Don't build for futures nobody asked for.
- Comments that repeat the code are useless; comments that explain why are valuable.
- Tests are required.

Where they disagree, and what these rules pick:

| Topic | *Clean Code* (Martin) | *Philosophy of Software Design* (Ousterhout) | These rules |
|---|---|---|---|
| Function length | 2 to 4 lines | Deep functions, length is secondary | Split by meaning, not line count ([11](11-size-functions-by-depth.md)) |
| Comments | Mostly a failure to express in code | Missing comments cost 10 to 100x more than wrong ones | Why-comments plus interface docs ([24](24-comments-explain-why.md)) |
| Decomposition | Extract until you can't | Over-splitting makes shallow, entangled methods | Inline pass-throughs ([25](25-deep-modules-hide-information.md)) |
| DRY | Strict | Repetition is a red flag, but so are bad abstractions | Rule of three, knowledge not text ([30](30-rule-of-three-wrong-abstraction.md)) |
| TDD | Required | Unit tests yes, TDD no | Tests required, process free ([32](32-test-behaviour-not-implementation.md)) |

## How strong is the evidence?

Every file ends with an evidence label. Most of software design is convention: taught by experienced people, rarely tested in controlled studies.

| Label | Meaning |
|---|---|
| Strong | Peer-reviewed study with a stated method, or official guidance from a language or large engineering org |
| Contested | Respected sources disagree; the file says which side it takes and why |
| Vendor | Published by a company that sells a related tool |
| Anecdote | Common review finding, no study |
| Convention | Widely taught by named practitioners, no controlled test found |

The few real studies: full-word names were about 19% faster to debug than abbreviations in one study of 72 developers (two others found no difference). Cognitive complexity correlates with comprehension time across about 24,000 ratings. Classes with code smells are more change-prone and fault-prone across 30 projects.

## Mindset

- [01 Follow the codebase before any rule here](01-follow-the-codebase-first.md)
- [02 Write for the next reader](02-write-for-the-reader.md)
- [03 Use Kent Beck's four rules, in order](03-four-rules-of-simple-design.md)
- [04 Build only what the task needs now](04-build-only-what-is-needed.md)

## Naming

- [05 Names say what a thing is, in full words](05-names-reveal-intent.md)
- [06 One word per concept, the domain's word](06-one-word-per-concept.md)
- [07 Booleans read as yes/no questions, in the positive](07-booleans-read-as-questions.md)
- [08 Functions are verbs, types are nouns, and verbs keep their meaning](08-verbs-for-functions-nouns-for-things.md)
- [09 Name your numbers and put units in the name](09-units-and-magic-numbers.md)
- [10 If it's hard to name, the design is wrong](10-hard-to-name-means-bad-design.md)
- [37 Naming conventions by language (reference)](37-naming-conventions-by-language.md)

## Functions

- [11 Size functions by depth, not by line count](11-size-functions-by-depth.md)
- [12 Handle the edge cases first and return early](12-guard-clauses-early-return.md)
- [13 Keep arguments few and never pass a bare boolean flag](13-few-arguments-no-flag-arguments.md)
- [14 Keep calculations pure and push I/O to the edges](14-separate-logic-from-side-effects.md)
- [15 Keep one level of detail per function](15-one-level-of-abstraction.md)
- [16 Don't mutate what you were given](16-dont-mutate-inputs.md)

## Control flow

- [17 Name complex conditions and keep control flow flat](17-keep-cognitive-complexity-low.md)
- [18 Boring code over clever code](18-boring-over-clever.md)

## Data and types

- [19 Parse input once at the boundary, then trust the types](19-parse-dont-validate.md)
- [20 Give important values their own types](20-avoid-primitive-obsession.md)
- [21 Decide where "nothing" is allowed, and say so in the type](21-handle-null-deliberately.md)

## Errors

- [22 Fail loudly, catch only where you can act, keep the cause](22-never-swallow-errors.md)
- [23 Design errors out of existence where you can](23-design-errors-away.md)

## Comments

- [24 Comments explain why; code explains what](24-comments-explain-why.md)

## Structure

- [25 Modules should be deep](25-deep-modules-hide-information.md)
- [26 Group files by feature, not by technical type](26-group-by-feature.md)
- [27 Business logic doesn't import the framework](27-dependencies-point-inward.md)
- [28 Prefer composition over inheritance](28-composition-over-inheritance.md)
- [29 One reason to change per file or class](29-keep-files-and-classes-focused.md)

## Duplication and dependencies

- [30 Prefer duplication over the wrong abstraction](30-rule-of-three-wrong-abstraction.md)
- [31 Reuse what exists before writing anything new](31-reuse-before-writing.md)

## Tests and changes

- [32 Test behaviour through the public interface](32-test-behaviour-not-implementation.md)
- [33 Keep changes small and separate tidying from behaviour](33-separate-tidying-from-behaviour.md)
- [34 Let tools enforce style](34-let-tools-enforce-style.md)

## For agents

- [35 Watch for the mistakes AI coding agents make most](35-ai-failure-modes.md)
- [36 Pre-commit checklist](36-pre-commit-checklist.md)

## Notes for building the skill

- Keep `SKILL.md` short: the priority order from rule 01, the one-line version of each rule, and the checklist from rule 36. Put these 37 files in a `references/` folder and tell Claude to open the matching file when a rule is in play. A 37-file wall loaded on every task wastes context.
- Make rule 01 the first line of the skill. Without it, the skill will fight existing codebases.
- Overlap to sort out first: the ponytail plugin already enforces rules 03, 04 and 31 (YAGNI, reuse, smallest diff), and its lazy bias pulls against rule 11 when deciding whether to extract. Either point the new skill at ponytail for those, or turn ponytail off where this skill is active. Two skills giving the same instruction in different words is a recipe for drift.
- Rules 12, 17, 21 and 34 can be enforced by tools (ESLint, PHPStan, Roslyn, SonarQube). The skill should run those tools, not just ask Claude to eyeball it.
- Test the skill on two or three real repos (one Laravel, one SvelteKit, one .NET) before trusting it everywhere.

## Sources

Books:

- Robert C. Martin, *Clean Code* (2008)
- John Ousterhout, *A Philosophy of Software Design* (2nd ed., 2021)
- Martin Fowler, *Refactoring* (2nd ed., 2018)
- Kent Beck, *Tidy First?* (2023)
- Andrew Hunt and David Thomas, *The Pragmatic Programmer* (1999)
- Eric Evans, *Domain-Driven Design* (2003)
- Joshua Bloch, *Effective Java* (3rd ed., 2018)
- Vladimir Khorikov, *Unit Testing* (Manning, 2020)

Studies:

- [Hofmeister, Siegmund, Holt: Shorter identifier names take longer to comprehend (SANER 2017)](https://www.se.cs.uni-saarland.de/publications/docs/HoSeHo17.pdf)
- [Never Work in Theory: abbreviated vs full-word names](https://neverworkintheory.org/2021/08/09/abbreviated-vs-full-names.html)
- [Muñoz Barón, Wyrich, Wagner: An Empirical Validation of Cognitive Complexity (ESEM 2020)](https://arxiv.org/abs/2007.12520)
- [Palomba et al.: On the diffuseness and the impact on maintainability of code smells (EMSE 2018)](https://research.tudelft.nl/en/publications/on-the-diffuseness-and-the-impact-on-maintainability-of-code-smel)
- [DevClass on GitClear's 2025 AI code quality report](https://devclass.com/2025/02/20/ai-is-eroding-code-quality-states-new-in-depth-report/)

Articles and guides:

- [Ousterhout and Martin debate: aposd-vs-clean-code](https://github.com/johnousterhout/aposd-vs-clean-code)
- [qntm: It's probably time to stop recommending Clean Code](https://qntm.org/clean)
- [Google engineering practices: what to look for in a code review](https://google.github.io/eng-practices/review/reviewer/looking-for.html)
- [Sandi Metz: The Wrong Abstraction](https://sandimetz.com/blog/2016/1/20/the-wrong-abstraction)
- [Kent C. Dodds: AHA Programming](https://kentcdodds.com/blog/aha-programming)
- [Martin Fowler: BeckDesignRules](https://martinfowler.com/bliki/BeckDesignRules.html)
- [Alexis King: Parse, don't validate](https://lexi-lambda.github.io/blog/2019/11/05/parse-don-t-validate/)
- [Gary Bernhardt: Functional Core, Imperative Shell](https://www.destroyallsoftware.com/screencasts/catalog/functional-core-imperative-shell)
- [SonarSource: Cognitive Complexity](https://www.sonarsource.com/blog/cognitive-complexity-because-testability-understandability/)
- [Ousterhout's red flags summarised](https://dev.to/sportebois/software-design-red-flags-wisdom-nuggets-from-john-ousterhout-43i2)
- [Jimmy Bogard: Vertical Slice Architecture](https://www.jimmybogard.com/vertical-slice-architecture/)
- [The Grug Brained Developer](https://grugbrain.dev/)
- [Microsoft .NET naming guidelines](https://learn.microsoft.com/en-us/dotnet/standard/design-guidelines/naming-guidelines)
- [Google TypeScript style guide](https://google.github.io/styleguide/tsguide.html)
- [PER Coding Style](https://www.php-fig.org/per/coding-style/)
