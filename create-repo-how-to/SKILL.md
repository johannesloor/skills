---
name: create-repo-how-to
description: Generate a repo-how-to skill that teaches agents how this repo's code is written now, from its recent changes.
disable-model-invocation: true
argument-hint: "[changes per area, default 10]"
---

# Create a repo how-to

You are the **coordinator**. Three stages run across real context boundaries: an explorer maps the areas, one area-writer per area reads that area's recent changes, you assemble. What the output looks like and what it caches are defined in `references/output-shape.md`; the stages' briefs are in `roles/`. If you can't dispatch subagents in isolated contexts here, stop and say so.

Every run is a full remake. The existing how-to is input only, read to keep wording that is still correct so the developer's diff stays small. You never commit; review and merge are the developer's.

Inputs: `N` changes per area (the argument, default 10) and the thin threshold `T` (default 2; the user may override it in the invocation).

## 0. Load the writing rules

Every agent in this run writes by `writing-for-agents`. If it is installed (`~/.claude/skills`, `~/.agents/skills`, or this repo's skill folders), read it there. Otherwise read the live files without saving them to disk:

- `https://raw.githubusercontent.com/mattpocock/skills/main/skills/productivity/writing-for-agents/SKILL.md`
- `https://raw.githubusercontent.com/mattpocock/skills/main/skills/productivity/writing-for-agents/SKILL-MECHANICS.md`

Pass this same lookup rule to every subagent. Done when you have read it; if neither source is reachable, stop and say so.

## 1. Explore

Dispatch one subagent with the brief in `roles/explorer.md`. Done when the area map is back and meets the explorer's completion criterion.

## 2. Write the areas

Dispatch one subagent per area, **in parallel**, with the brief in `roles/area-writer.md`. Done when every area has returned a reference or been reported thin with no file.

## 3. Write the overview

Build `.agents/skills/repo-how-to/SKILL.md` in the shape from `references/output-shape.md`. A rule reported as repo-wide by two or more areas moves up into Repo-wide rules and leaves those references — and passes the cache test: a rule already visible in the files it governs stays out, however repo-wide it is. Start from the existing overview: lines still true keep their exact wording and order. Done when every area with a reference has an index line and the description has one trigger per such area.

## 4. Remake the files

Write every reference to `.agents/skills/repo-how-to/references/`. Delete any `references/*.md` whose area is gone or now thin with no file. Done when the folder holds exactly the overview and the references the index names.

## 5. Symlink

```bash
mkdir -p .claude/skills && ln -sfn ../../.agents/skills/repo-how-to .claude/skills/repo-how-to
```

Done when `ls .claude/skills/repo-how-to/` lists the overview.

## 6. Pointer line

Add the pointer line from `references/output-shape.md`, following the explorer's `Agent file layout`: when one agent file imports or symlinks the other, only the real file gets it; otherwise each existing file does; when neither exists, create `AGENTS.md`. A line already mentioning `repo-how-to skill` is replaced in place. Done when `grep -c 'repo-how-to skill' <file>` returns 1 for each target file.

## 7. Prune

Do a `writing-for-agents` pass over the overview and every reference: cut no-ops, duplicates, caches of what the environment already says, and anything from the README or agent files. Done when every remaining line answers "what does this give an agent that exploring won't?"

## 8. Report and stop

The tree is the deliverable; leave it uncommitted. Report:

```
## Areas
<area — reference | thin, no file | thin, short file>

## Changed files
<git status --short>
```
