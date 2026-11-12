---
name: update-repo-how-to
description: Refresh this repo's repo-how-to skill from the changes since its last run.
disable-model-invocation: true
argument-hint: "[changes per area, default 10]"
---

# Update a repo how-to

You are the **coordinator**. Three stages run across real context boundaries: an explorer maps the areas, one area-writer per area reads that area's recent changes, you assemble. What the output looks like and what it caches are defined in `references/output-shape.md`; the stages' briefs are in `roles/`. If you can't dispatch subagents in isolated contexts here, stop and say so.

Inputs: `N` changes per area (the argument, default 10) and the thin threshold `T` (default 2; the user may override it in the invocation).

## 0. Load the writing rules

Every agent in this run writes by `writing-for-agents`, installed in this repo's skill folders by `create-repo-how-to`. Read it there and pass its path to every subagent. Done when you have read it; if it isn't installed, stop and say so.

## 1. Explore

Dispatch one subagent with the brief in `roles/explorer.md`. Done when the area map is back and meets the explorer's completion criterion.

## 2. Write the areas

Record this run's read-through commit: `git rev-parse <branch>`, with `<branch>` the default branch from the explorer's `Remote`.

An area reads `since <sha>` when all of these hold; otherwise it reads its last `N` changes:

- the run is incremental (every run is, except when `create-repo-how-to` runs you as a full remake);
- the existing overview has a `<!-- read-through: <sha> -->` line, and `git merge-base --is-ancestor <sha> <branch>` succeeds;
- the area has an existing reference that doesn't start with `> Thin:`.

A `since` area with no commits in `git log <sha>..<branch> -- <pathspecs>` (its scope globs, each prefixed `:(glob)`) is **kept**: its reference stays as it is and it gets no subagent. Dispatch one subagent per other area, **in parallel**, with the brief in `roles/area-writer.md`. An area that returns `unchanged` is kept too. Done when every area has a kept reference, has returned a reference, or has been reported thin with no file.

## 3. Write the overview

Build `.agents/skills/repo-how-to/SKILL.md` in the shape from `references/output-shape.md`. Start from the existing overview: lines still true keep their exact wording and order, so the developer's diff stays small; sections the shape doesn't define go. Done when the overview holds only the shape's sections, every area with a reference has an index line, the description matches the shape's exact wording, and the read-through line holds this run's read-through commit.

## 4. Remake the files

Write every returned reference to `.agents/skills/repo-how-to/references/`; a kept reference stays untouched. Delete any `references/*.md` whose area is gone or, after reading its last `N`, now thin with no file. Done when the folder holds exactly the overview and the references the index names.

## 5. Prune

Do a `writing-for-agents` pass over the overview and every reference written this run: cut no-ops, duplicates, caches of what the environment already says, and anything from the README or agent files. Done when every remaining line answers "what does this give an agent that exploring won't?"

## 6. Report

```
## Areas
<area — reference | kept, no new changes | thin, no file | thin, short file>

## Changed files
<git status --short>
```
