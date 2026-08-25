# skills

Agent skills.

## implement-plan

Executes an approved plan through a pipeline of isolated subagents — implementer, test-writer, critic, mutation tester, and plan-verifier — each seeing only what its own job requires, all communicating through a coordinator.

The point is that no agent grades its own homework: the test-writer never sees the implementation, and the critic never sees the plan.

### Install

Clone, then symlink into your agent's skills directory:

```bash
git clone https://github.com/johannesloor/skills.git ~/.agents/skills-repo
ln -s ~/.agents/skills-repo/implement-plan ~/.claude/skills/implement-plan
```
