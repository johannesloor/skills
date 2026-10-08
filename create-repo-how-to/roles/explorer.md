# Role: explorer

Dispatched once, before any area is written. Hand it the brief below, filled in.

```
Map the coding areas of the repository at <repo root>. You only map; other agents
write about each area afterwards, from your map.

Read: the README, AGENTS.md and CLAUDE.md, CONTRIBUTING*, docs/, every manifest
(package.json and its workspaces, pnpm-workspace.yaml, pyproject.toml, Cargo.toml,
go.mod, and so on), and the top two levels of the directory tree.
Existing how-to: <path to .agents/skills/repo-how-to/, or "none">. If there is one,
read its SKILL.md and reuse its area names where they still fit.

Take areas from that structure. Commit history is out of scope for this job.

An area is a part of the repo where code is written in a distinct way. A family of
similar units (every middleware, every adapter, every migration) is one area, not
one per unit. Repo tooling that ships to no user (build scripts, benchmarks, perf
measurement, cross-runtime test harnesses) is one area together. Draw the fewest
areas that keep each one's way of writing uniform; most repos have 3 to 10. Name
each area with the word the repo itself uses: a README heading, a folder name, a
package name.

Return Markdown with these sections:

## Areas
For each area:
- **<name>**: <one line on what kind of code lives there>
  - Scope: <git glob pathspecs: * stays in one folder, ** crosses folders>
  - Triggers: <the repo's own words for the work done there, most common first>

## Not an area
<path>: <reason: generated, vendored, fixtures, docs-only, config, ...>

## Already in agent files
<each rule AGENTS.md / CLAUDE.md / CONTRIBUTING already states, one line each, so
nobody repeats it>

## Agent file layout
<which of AGENTS.md and CLAUDE.md exist; whether one imports the other (@AGENTS.md)
or is a symlink to it; which one is the real file>

## Remote
Default branch: <name>. GitHub: <owner/repo, or "none">.

Done when every top-level source directory and every workspace package belongs to
exactly one area or is listed under "Not an area" with its reason.
```

Writing rules: append the coordinator's `writing-for-agents` lookup rule to the brief.
