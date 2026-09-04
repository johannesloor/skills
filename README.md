# skills

Agent skills.

## implement-plan

Executes an approved plan through a pipeline of isolated subagents — specifier, implementer, test-writer, cleaner, hardener, QA, and plan-verifier — each seeing only what its own job requires, all communicating through a coordinator.

The point isn't to skip review — it's to make review worth your time. What reaches you has already survived every mechanical objection, so your attention goes to design and judgement rather than to catching things a machine could have caught. Every stage exits on a fact rather than an opinion: the suite is green, the cleaner is silent, every mutant is killed or argued for, the QA steps passed against the running system.

And no agent is ever in a position to grade its own work — the specifier freezes acceptance criteria before any code exists, the test-writer never sees the implementation, the cleaner never sees the plan, and the QA agent never sees any source at all.

Inspired by the agent pipeline Robert C. Martin describes in [his conversation with Matt Pocock](https://www.youtube.com/live/zcLPGC-tvgk) — the specifier, cleaner, and hardener roles are his.

## visualize

Puts a plan, an implementation, or both onto a shared canvas — a whiteboard or a design file — for a team to stand in front of and argue with.

Tool-agnostic: you give it a link, it works through whatever MCP server serves that tool, and walks you through setting one up when none is attached. The medium is read from the link itself, because a collaborative whiteboard and a precise design canvas want genuinely different artifacts — one built from pieces people can drag, the other from cards people read.

The discipline that matters is tense. A plan and a result are different artifacts, and a plan drawn with hindsight is the easy mistake to make, because the output still looks correct. So a plan gets drawn as a plan: unknowns stay unknown, every branch stays live, and a gate marks where work stops and waits for a person — whatever the agent happens to know about how it actually went.

Overview beats detail, but nothing silently vanishes: the source is broken into numbered units — bands, timelines, swimlanes, journeys, maps, mixed freely as the material demands — and compressed by translating prose into shape rather than into shorter prose. A paragraph describing three stages and a dependency is three boxes and an arrow; the words that survive become labels. Size, position, proximity, connectors and colour all carry meaning, so a unit that still needs a paragraph to explain itself is a diagram not yet found. Each board also gets at least one diagram the source could not draw — the bind, the fork, the story map, the impact matrix that prose spreads across several paragraphs and a picture states at once. Then it checks its own work: element bounds for overlaps and clipped text, a render to look at, and a squint at a zoom where the words are unreadable, because an argument that only survives in the text is one the board is not making.

## Install

```bash
npx skills add johannesloor/skills
```

Or pick the agent and scope explicitly:

```bash
npx skills add johannesloor/skills --skill implement-plan -g -a claude-code
```
