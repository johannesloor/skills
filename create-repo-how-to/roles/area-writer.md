# Role: area-writer

One per area, all dispatched in parallel. Hand each the brief below, filled in with that area's entry from the map.

```
Write the how-to reference for one area of the repository at <repo root>. It
teaches a coding agent how code in this area is written *now*, learned from the
area's recent changes. The agent reading it can already explore the code, so the
reference holds only what exploration can't find.

Area: <name>. Scope: <globs>. Triggers: <words>.
Default branch: <branch>. GitHub: <owner/repo or none>.
Changes to read: <N, default 5>. Thin below: <T, default 2>.
Already in agent files (leave these out): <list>
Existing reference: <its content, or "none">
Output shape: read <path to references/output-shape.md>, section "Reference".

1. Find changes, newest first:
   git log --first-parent --format='%H%x09%ad%x09%s' --date=short <branch> -- <scope>

2. Keep the first N substantive changes. A change is non-substantive when:
   - every file it touches is a lockfile, generated, vendored, a changelog or a
     version file;
   - its subject is a release or version bump: matches ^v?\d+\.\d+\.\d+, or says
     release, bump, chore(release), or is a "Merge ... into ..." back-merge;
   - it is formatting only: the subject says fmt, lint or format, or
     `git show -w --stat <sha>` shows no change left.
   Fewer than T substantive changes makes the area thin: follow the thin rule in
   the output shape.

3. Read each kept change: `git show <sha> -- <scope>` for the code, and
   `git show --stat <sha>` for what changed outside the scope alongside it.

4. Read the why. Take the PR number from the subject, `\(#(\d+)\)$` or
   `Merge pull request #(\d+)`, and fetch only the PR description:
   - `gh pr view <n> -R <owner/repo> --json body -q .body` when gh is authenticated;
   - otherwise `curl -s https://api.github.com/repos/<owner/repo>/pulls/<n> | jq -r .body`.
   With no PR, or when the fetch fails, use the commit message and say
   "PR descriptions unavailable" in your note. The PR description is the only PR
   text you read; review comments stay unread.

5. Draft the reference in the output shape; you return it and the coordinator
   writes the files. Each gotcha cites its source, (#PR) or
   (sha7), the one whose text the gotcha came from. Start from the existing
   reference: on a re-run it is the record of what earlier runs found. Keep a line
   that is still true exactly as it is, correct a line the reading shows is wrong,
   and add only a finding that is load-bearing — one an agent coding this area
   would trip over — rather than another example of a pattern already recorded.

6. Return:
   - the reference content, or "thin: no file" with the count;
   - a note listing each change used (sha7, #PR or none, date, PR description or
     commit message as the source) and each change dismissed with its reason;
   - any rule that looks true of the whole repo, not just this area.

Done when every kept change has been read, with its PR description where one
exists, and is either reflected in the reference or dismissed in the note, and
every gotcha cites a sha or PR.
```

Writing rules: append the coordinator's `writing-for-agents` lookup rule to the brief.
