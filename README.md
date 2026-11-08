# Clean code rules

Landing page: https://abdulkader-safi.github.io/Clean-Code-Rules/

A Claude Code plugin that makes Claude write code the next person can read and change. It has 36 rules plus a naming reference for each language, built from Ousterhout, Martin, Beck, Fowler, Google's review guide and a handful of real studies.

The first rule is the one that matters most: Claude reads your codebase before it writes, and your project's conventions beat every rule here.

## Install

In Claude Code:

```
/plugin install clean-code --marketplace Abdulkader-Safi/Clean-Code-Rules
```

On versions before 2.1.275, add the marketplace first:

```
/plugin marketplace add Abdulkader-Safi/Clean-Code-Rules
/plugin install clean-code@clean-code-rules
```

To try it from a local clone without installing:

```
claude --plugin-dir /path/to/Clean-Code-Rules
```

## Use

The skill loads on its own when Claude writes, edits, refactors or reviews code. To call it by hand:

```
/clean-code:clean-code review my last change
```

On each task Claude will:

1. Read nearby files and match the project's naming, error handling and test style.
2. Apply the rules, opening a rule's full file only when it needs the examples.
3. Run your project's formatter, linter, type checker and tests.
4. Check its own diff against 12 common AI coding mistakes and a pre-commit checklist, then delete what they catch.

## What the rules cover

| Area | Rules |
|---|---|
| Mindset | 01 to 04 |
| Naming | 05 to 10, plus 37 (conventions by language) |
| Functions | 11 to 16 |
| Control flow | 17, 18 |
| Data and types | 19 to 21 |
| Errors | 22, 23 |
| Comments | 24 |
| Structure | 25 to 29 |
| Duplication | 30, 31 |
| Tests and changes | 32 to 34 |
| For agents | 35 (AI failure modes), 36 (pre-commit checklist) |

Where *Clean Code* and *A Philosophy of Software Design* disagree, the rules mostly side with Ousterhout: size functions by meaning instead of line count, write comments that explain why, and wait for the third copy before abstracting. [research/README.md](research/README.md) has the full comparison and the evidence label on every rule.

Examples are mostly TypeScript, with C#, PHP and SQL where a rule differs by language.

## Layout

```
.claude-plugin/
  plugin.json          plugin manifest
  icon.png             listing icon
  marketplace.json     lets the repo act as its own marketplace
skills/clean-code/
  SKILL.md             priority order, one line per rule, final checks
research/              one file per rule: what, why, check, bad, good, fix, sources
docs/index.html        the landing page, served by GitHub Pages
```

`SKILL.md` stays short so it doesn't fill Claude's context on every task. The detail lives in `research/`, and Claude opens one file at a time.

## Ponytail

If you also run the ponytail plugin, both enforce rules 03, 04 and 31 (build less, reuse first). They agree, so there's no conflict. Ponytail's urge to inline can pull against rule 11 when deciding whether to extract a function. Rule 11 says split by meaning, so a named step that hides real work stays a function.

## License

MIT
