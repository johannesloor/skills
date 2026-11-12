---
name: create-repo-how-to
description: Set up a repo-how-to skill in this repo, point agents at it, and offer a weekly update.
disable-model-invocation: true
argument-hint: "[changes per area, default 10]"
---

# Create a repo how-to

One-time setup. `update-repo-how-to` builds and refreshes the how-to; this skill installs it in the repo, runs it once, wires the result into the repo, and offers to schedule it.

## 0. Install the skills

Install `update-repo-how-to` and the `writing-for-agents` it writes by, at project level, so a scheduled job finds them in the checkout:

```bash
npx skills add johannesloor/skills --skill update-repo-how-to -y
npx skills add mattpocock/skills --skill writing-for-agents -y
```

Done when this repo's skill folders hold `update-repo-how-to/SKILL.md` and `writing-for-agents/SKILL.md`, and you have read update's; if an install fails, stop and say so.

## 1. Build the how-to

Follow `update-repo-how-to` as its coordinator, as a **full remake**, passing on `N` and `T` from this invocation. Done when its report is ready.

## 2. Symlink

```bash
mkdir -p .claude/skills && ln -sfn ../../.agents/skills/repo-how-to .claude/skills/repo-how-to
```

Done when `ls .claude/skills/repo-how-to/` lists the overview.

## 3. Pointer line

Add this line, exactly:

`For how to write code in a specific area of this repo, use the repo-how-to skill (.agents/skills/repo-how-to/).`

Follow the `Agent file layout` from step 1's area map: when one agent file imports or symlinks the other, only the real file gets it; otherwise each existing file does; when neither exists, create `AGENTS.md`. A line already mentioning `repo-how-to skill` is replaced in place. Done when `grep -c 'repo-how-to skill' <file>` returns 1 for each target file.

## 4. Report

Print update's report, with the installed skills, symlink and pointer-line files included in `## Changed files`.

## 5. Offer a weekly update

Skip this step when a job already runs `update-repo-how-to` for this repo.

Ask the user whether to set up a weekly update. If yes, ask how they want it scheduled on their system, offering what fits the system. Set up the one they pick. The job runs `update-repo-how-to`, and its prompt carries what an unattended run needs:

- `N` and `T`, when they differ from the defaults;
- the default branch, with full history checked out;
- how area-writers read PR descriptions there when `gh` has no access;
- what to do with the changes: open a pull request or leave them in the tree, as the user chooses.

Done when the job exists and you have given the user the command that triggers it by hand, or the user declined.
