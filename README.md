# skills

Agent skills.

## implement-plan

Executes an approved plan through a pipeline of isolated subagents — specifier, implementer, test-writer, cleaner, hardener, QA, and plan-verifier — each seeing only what its own job requires, all communicating through a coordinator.

The point isn't to skip review — it's to make review worth your time. What reaches you has already survived every mechanical objection, so your attention goes to design and judgement rather than to catching things a machine could have caught. Every stage exits on a fact rather than an opinion: the suite is green, the cleaner is silent, every mutant is killed or argued for, the QA steps passed against the running system.

And no agent is ever in a position to grade its own work — the specifier freezes acceptance criteria before any code exists, the test-writer never sees the implementation, the cleaner never sees the plan, and the QA agent never sees any source at all.

Inspired by the agent pipeline Robert C. Martin describes in [his conversation with Matt Pocock](https://www.youtube.com/live/zcLPGC-tvgk) — the specifier, cleaner, and hardener roles are his.

### Install

```bash
npx skills add johannesloor/skills
```

Or pick the agent and scope explicitly:

```bash
npx skills add johannesloor/skills --skill implement-plan -g -a claude-code
```
