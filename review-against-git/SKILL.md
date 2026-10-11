---
name: review-against-git
description: Review a code change against the repo's git history and fix what conflicts with how the team builds. Use after implementing a code change, before building, testing or presenting it; or on a branch, commit range or PR.
argument-hint: "[commit range | PR number]"
---

# Review against git

You are the **coordinator**. The change you just made is checked against the history of the code it touches: why that code is the way it is, as told by its commits, PRs and reviewers. A reviewer reads the history and returns findings with **evidence**; you apply them.

The run sits between implementing and building: implement → review → apply → build/test → present.

## 1. Classify

Changes to only docs or comments, and mechanical changes (renames done by tooling, dependency bumps), need no review: stop here and leave the report out. Every other code change goes on. Done when the change is classified.

## 2. Fix the target

The target is one diff and its **left revision**: the code as it was before the change. The reviewer reads history only up to the left revision, so the change never counts as evidence for itself.

- No argument: the uncommitted changes, `git diff HEAD`, plus untracked files. Left: `HEAD`.
- No argument and a clean tree: the branch against its base. Left: `git merge-base HEAD <default branch>`, with the default branch from `git symbolic-ref --short refs/remotes/origin/HEAD`, else `gh repo view --json defaultBranchRef -q .defaultBranchRef.name`, else ask the user.
- A commit range `A..B`: `git diff A..B`. Left: `A`.
- A PR number: `gh pr diff <n>`. Left: `gh pr view <n> --json baseRefOid -q .baseRefOid`.

Done when the diff and its left revision are both known.

## 3. Gather the brief's inputs

- Every **hunk**: its file and its lines on the left side as `start,end`. From the header `-a,b`: start `a`, end `a+b-1`, and a missing `b` means 1. A hunk that only inserts (`b` is 0) takes `a-2` to `a+3`, clipped to the file.
- New files, untracked ones included, listed by path.
- GitHub: `owner/repo` when `gh auth status` succeeds and the remote is on GitHub; otherwise none.
- Each explicit request from the user that the diff carries out, quoted.

Done when every hunk and new file of the target is listed.

## 4. Review

Dispatch one fresh subagent with the brief in `roles/reviewer.md`, filled in. A fresh context reads the history without your bias toward the code you wrote, and keeps that reading out of your context. When you can't dispatch a subagent here, run the brief yourself. Done when the findings are back and meet the brief's completion criterion; zero findings is a valid result.

## 5. Apply

Check each finding's quote against its cited source (`git log -1 --format=%B <sha>`, the PR, or the doc); a quote you can't find there drops the finding. Apply each remaining finding as exactly the change it names, when the target's changed code is what is checked out here; for a range or PR that isn't, report each finding instead. A finding that would undo something the user explicitly asked for is left unapplied: the user wins, and it is reported as a conflict with its evidence. The lines you change here are final for this run; they get reviewed again only when later work changes them. Done when every finding is checked, then applied, reported, dropped or recorded as a conflict.

## 6. Report

Go on to build and test. When you present the change, include:

```
Reviewed against git: <N> fixes applied
- <file:line> — <what changed> (<#PR, sha7 or doc path>: "<quote>")
Conflicts with your request:
- <what you asked> vs <what history says> (<#PR, sha7 or doc path>: "<quote>")
```

`N` counts the findings applied, one line each. Leave out the conflicts section when there are none. When the reviewer's `GitHub read` is not yes, add that PRs weren't fully read, why, and how to enable it: `gh auth login`, or that the remote isn't on GitHub.
