# skills

Agent skills.

## implement-plan

Executes an approved plan through a pipeline of isolated subagents — specifier, implementer, test-writer, cleaner, hardener, QA, and plan-verifier — each seeing only what its own job requires, all communicating through a coordinator.

The point isn't to skip review — it's to make review worth your time. What reaches you has already survived every mechanical objection, so your attention goes to design and judgement rather than to catching things a machine could have caught. Every stage exits on a fact rather than an opinion: the suite is green, the cleaner is silent, every mutant is killed or argued for, the QA steps passed against the running system.

And no agent is ever in a position to grade its own work — the specifier freezes acceptance criteria before any code exists, the test-writer never sees the implementation, the cleaner never sees the plan, and the QA agent never sees any source at all.

Inspired by the agent pipeline Robert C. Martin describes in [his conversation with Matt Pocock](https://www.youtube.com/live/zcLPGC-tvgk) — the specifier, cleaner, and hardener roles are his.

## visualize

Puts whatever a team needs to look at together — a plan, a shipped implementation, a roadmap, an architecture, a retrospective — onto a shared canvas, whiteboard or design file, for them to stand in front of and argue with.

Tool-agnostic: you give it a link, it works through whatever MCP server serves that tool, and walks you through setting one up when none is attached. The medium is read from the link itself, because a collaborative whiteboard and a precise design canvas want genuinely different artifacts — one built from pieces people can drag, the other from cards people read.

When what you point it at is a plan, one discipline kicks in: tense. A plan and a result are different artifacts, and a plan drawn with hindsight is the easy mistake to make, because the output still looks correct. So a plan gets drawn as a plan: unknowns stay unknown, every branch stays live, and a gate marks where work stops and waits for a person — whatever the agent happens to know about how it actually went.

Overview beats detail, but nothing silently vanishes: the source is broken into numbered units — bands, timelines, swimlanes, journeys, maps, mixed freely as the material demands — and compressed by translating prose into shape rather than into shorter prose. A paragraph describing three stages and a dependency is three boxes and an arrow; the words that survive become labels. Size, position, proximity, connectors and colour all carry meaning, so a unit that still needs a paragraph to explain itself is a diagram not yet found. Each board also gets the diagrams the source could not draw — the bind, the fork, the story map, the impact matrix that prose spreads across several paragraphs and a picture states at once. Then it checks its own work: element bounds for overlaps and clipped text, a render to look at, and a squint at a zoom where the words are unreadable, because an argument that only survives in the text is one the board is not making.

## create-repo-how-to

Run in any repository, it writes a `repo-how-to` skill into it — a short overview plus one reference per coding area, in `.agents/skills/repo-how-to/` with a `.claude/skills` symlink — and adds one line to `AGENTS.md` or `CLAUDE.md` pointing agents at it.

It doesn't summarise the code; an agent can already read the code. It learns from each area's last few substantive changes and why they were made, and caches only what exploring won't turn up: which way an area is moving and which patterns are legacy, which files change together, and the traps recent fixes exposed — each one cited to the commit or PR it came from.

Every run is a full remake, never committed. The previous version is read only to keep wording that's still true, so a re-run shows up as a small `git diff` you review like any other change.

## review-against-git

After an agent implements a change, and before it builds, tests or shows you anything, a fresh subagent reviews the diff against the git history of the code it touches. It finds what conflicts with how the team builds things, and the agent fixes it, so the first code you review has already been held up against the team's own past decisions.

It reads history at review time, for the lines actually changed, rather than from a cached "how we code" document. A cached document goes stale (a fix gets reverted, a rule narrows to one platform) and turns a single PR into a rule. Reading at review time sees HEAD as it is and only the history that bears on the diff. Two histories are read for each changed file. The changed lines' own history shows which past decisions not to undo. The newest files with the same role (same interface, base class or type suffix in the same layer, whichever folder they sit in) show how that kind of code is written now. Each commit is read only as deep as needed: commit message, then PR description, then review comments. The repo's own convention docs and skills count as evidence too.

A finding needs evidence: a stated reason ("we do X because…") in a commit, PR or human review comment, or two or more independent changes doing the same thing. The pattern must also still hold at HEAD. One unexplained commit is never enough, because a false finding costs more than a missed one. When history disagrees with something you explicitly asked for, you win, and the conflict is reported with its evidence. Without GitHub it falls back to commit messages and repo docs, and says so.

It is model-invoked, but the most reliable trigger is a line in each repo's `AGENTS.md` or `CLAUDE.md`:

```
Before presenting code changes, run the review-against-git skill.
```

## Install

```bash
npx skills add johannesloor/skills
```

Or pick the agent and scope explicitly:

```bash
npx skills add johannesloor/skills --skill implement-plan -g -a claude-code
```
