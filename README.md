# skills

Agent skills.

## implement-plan

Executes an approved plan through a gauntlet of isolated subagents — specifier, implementer, test-writer, cleaner, hardener, QA, and plan-verifier — each seeing only what its own job requires, all communicating through a coordinator.

The point is that no agent grades its own homework. The specifier freezes acceptance criteria before any code exists, the test-writer never sees the implementation, the cleaner never sees the plan, and the QA agent never sees any source at all.

Inspired by Robert C. Martin's agent pipeline, discussed with Matt Pocock.

### Install

Clone, then symlink into your agent's skills directory:

```bash
git clone https://github.com/johannesloor/skills.git ~/.agents/skills-repo
ln -s ~/.agents/skills-repo/implement-plan ~/.claude/skills/implement-plan
```
