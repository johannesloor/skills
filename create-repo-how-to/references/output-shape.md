# Output shape

What `create-repo-how-to` writes into the target repo. Everything lives under `.agents/skills/repo-how-to/`.

Every line is a **cache**: it holds only what an agent can't find by reading the code, the manifests, the README or the agent files. Point to paths; never paste code.

## Overview: `SKILL.md`

```markdown
---
name: repo-how-to
description: How to write code in <repo>. Use when <one trigger per area, in the repo's own words>.
---

## Repo-wide rules
- <rule that holds in every area and that exploration can't reveal> (<#PR or sha7>)

## Areas
- **<area>** (`<scope globs>`): <when to read it>. See [references/<area>.md](references/<area>.md)
```

- 50 lines at most.
- One trigger per area in the description. Collapse synonyms into one.
- Scope lives only in the index line, so references don't repeat it.
- Leave out setup, architecture and orientation prose. Leave out anything the README or `AGENTS.md`/`CLAUDE.md` already says; point to it only when an area depends on it.
- A thin area with no reference gets no index line and no trigger.
- Leave out `## Repo-wide rules` when there are none.

## Reference: `references/<area>.md`

File name: the area name, lowercase, hyphenated. Sections in this order; leave out any section with nothing to say.

- `## Current pattern`: the direction the area is moving, with one or two example file paths to copy from. Older patterns still in the code go under `Legacy (don't copy):`, each naming what replaced it.
- `## Change together`: for each common kind of change, the set of files the recent diffs changed together.
- `## Gotchas`: the why, the unwritten rule, the trap a fix exposed. Each one ends with its source: `(#PR)` or `(sha7)`.
- `## Verify`: only when the command isn't obvious from the manifests.

**Thin area:** fewer than 2 substantive changes. Write no file, or start the file with `> Thin: based on N change(s).`

## Pointer line

One line in the agent file, exactly:

`For how to write code in a specific area of this repo, use the repo-how-to skill (.agents/skills/repo-how-to/).`
