# skills

Agent skills.

## implement-plan

Executes an approved plan through a pipeline of isolated subagents — specifier, implementer, test-writer, cleaner, hardener, QA, and plan-verifier — each seeing only what its own job requires, all communicating through a coordinator.

The point is that no agent grades its own homework. The specifier freezes acceptance criteria before any code exists, the test-writer never sees the implementation, the cleaner never sees the plan, and the QA agent never sees any source at all.

Inspired by the agent pipeline Robert C. Martin describes in [his conversation with Matt Pocock](https://www.youtube.com/live/zcLPGC-tvgk) — the specifier, cleaner, and hardener roles are his.

### Install

```bash
npx skills add johannesloor/skills
```

Or pick the agent and scope explicitly:

```bash
npx skills add johannesloor/skills --skill implement-plan -g -a claude-code
```
