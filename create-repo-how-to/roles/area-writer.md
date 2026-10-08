# Role: area-writer

One per area, all dispatched in parallel. Hand each the brief below, filled in with that area's entry from the map.

```
Write the how-to reference for one area of the repository at <repo root>. It
teaches a coding agent how code in this area is written *now*, learned from the
area's recent changes. The agent reading it can already explore the code, so the
reference holds only what exploration can't find.

Area: <name>. Scope: <globs>. Triggers: <words>.
Default branch: <branch>. GitHub: <owner/repo or none>.
Changes to read: <N, default 10>. Thin below: <T, default 2>.
Already in agent files (leave these out): <list>
Existing reference: <its content, or "none">
Output shape: read <path to references/output-shape.md>, section "Reference".

Scope globs are git glob pathspecs: `*` stays inside one folder, `**` crosses
folders. Pass each one prefixed with `:(glob)`, written below as <pathspecs>.

1. Find changes, newest first:
   git log --first-parent --format='%H%x09%ad%x09%s' --date=short <branch> -- <pathspecs>

2. Keep the first N substantive changes. A change is non-substantive when:
   - every file it touches is a lockfile, generated, vendored, a changelog or a
     version file;
   - its subject is a release or version bump: matches ^v?\d+\.\d+\.\d+, or says
     release, bump, chore(release), or is a "Merge ... into ..." back-merge;
   - it is formatting only: the subject says fmt, lint or format, or
     `git diff -w --stat <sha>^ <sha> -- <pathspecs>` shows no change left.
   Fewer than T substantive changes makes the area thin: follow the thin rule in
   the output shape.

3. Read each kept change against its first parent, which works the same for
   squash and merge commits:
   - `git diff <sha>^ <sha> -- <pathspecs>` for the code;
   - `git diff --stat <sha>^ <sha>` for what changed outside the scope alongside it.
   When `git diff --shortstat <sha>^ <sha> -- <pathspecs>` shows more than 1,500
   changed lines (a mass rename, a migration, a vendored drop), read only the
   stat and the why for that change: its full diff would crowd out the others.

4. Read the why. Start with the commit message: `git log -1 --format=%B <sha>`.
   When its body says why the change was made, that is the source. A subject that
   only names the change does not explain it. Otherwise, when the subject carries
   a PR number, `\(#(\d+)\)$` or `Merge pull request #(\d+)`, fetch that PR's
   description, and only the description; review comments stay unread:
   - `gh pr view <n> -R <owner/repo> --json body -q .body` when gh is authenticated;
   - otherwise `curl -s https://api.github.com/repos/<owner/repo>/pulls/<n> | jq -r '.body // empty'`.
   An empty result, an API error, or a body that is only the PR template's
   checklist counts as no description. With no description, the why is whatever
   the diff itself shows; don't invent one.

5. Draft the reference in the output shape; you return it and the coordinator
   writes the files. Each gotcha ends with its citation: (#PR) when the change
   has a PR number, otherwise (sha7). Start from the existing reference: on a
   re-run it is the record of what earlier runs found. Keep a line that is still
   true exactly as it is, correct a line the reading shows is wrong, and add only
   a finding that is load-bearing — one an agent coding this area would trip
   over — rather than another example of a pattern already recorded.

6. Return:
   - the reference content, or "thin: no file" with the count;
   - a note listing each change used (sha7, #PR or none, date) and each change
     dismissed with its reason;
   - any rule that looks true of the whole repo, not just this area.

Done when every kept change has been read, its why looked for as step 4 says,
and is either reflected in the reference or dismissed in the note, and every
gotcha ends with a (#PR) or (sha7) citation.
```

Writing rules: append the coordinator's `writing-for-agents` lookup rule to the brief.
