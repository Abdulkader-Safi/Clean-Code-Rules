# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this repo is

A Claude Code plugin named `clean-code`. It has no build step and no code: it's a skill plus markdown rule files. The repo is also its own marketplace (`clean-code-rules`), so users install straight from GitHub (`Abdulkader-Safi/Clean-Code-Rules`).

## Commands

```sh
claude plugin validate .            # check manifests, skill frontmatter and README install line
claude --plugin-dir .               # load the plugin locally to try it
```

## How the pieces connect

- `docs/index.html` is the landing page (GitHub Pages, served from `main` `/docs`). One static file, GSAP from cdnjs with SRI hashes, ink-lime design tokens. Keep its rule list in sync with `research/`.

- `.claude-plugin/plugin.json` is the manifest. `.claude-plugin/marketplace.json` lists the same plugin with `"source": "./"`. Bump `version` in plugin.json when the rules change.
- `skills/clean-code/SKILL.md` is the only file loaded into context. It holds the priority order, one line per rule and the final checks. Keep it short.
- `research/` holds the full rules, one per file, numbered `01` to `37`, plus `README.md` with the evidence labels and sources. SKILL.md points to them by relative path (`<skill base>/../../research/NN-name.md`), so Claude opens one only when that rule is in play. There is a single copy: don't duplicate these files into the skill folder.
- `research/` was copied from `~/Documents/Cowork/Knowledge/Research/clean-code`. If a rule changes there, copy it over again.

## When editing rules

- Adding, renaming or renumbering a rule file means updating three places: the filename, its line in `SKILL.md`, and the index in `research/README.md`. Rule 36 (checklist) and rule 35 (AI failure list) also cite rules by number.
- Each rule file follows the same shape: what it is, why it works, check, bad, good, fix, sources, evidence label.
- Rule 01 (the codebase wins) must stay first in SKILL.md.

## Writing style

All prose (README, SKILL.md, rule files) follows the anti-ai-writing rules: no em dashes, sentence-case headings, no lists of exactly three. The ai-writing-rules plugin hook checks every `.md` write and blocks on violations.
