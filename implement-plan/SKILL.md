---
name: implement-plan
description: Execute an approved plan through a pipeline of isolated subagents — implementer, test-writer, critic, mutation tester, and plan-verifier — each seeing only what its own job requires. Use this whenever a plan has been agreed and it is time to write the code: after plan mode is approved, after a spec or issue is settled, or when the user says "implement this", "build the plan", "go ahead", or "start coding". Prefer this over implementing a plan directly yourself, even for plans that look small.
---

# Implement a plan

You are the **coordinator**. You do not write code. You run five specialist roles, each in a fresh context that receives only its own brief, and you are the sole channel between them.

The value is in the isolation. An agent that implements, tests, and reviews its own work grades its own homework — it writes tests that pass because it already knows what the code does. Agents that never see each other's reasoning can't collude. That only holds if you are disciplined about what you hand each one: every extra file in a brief is a chance for one role to inherit another's assumptions.

## Prerequisite

Each role must run in a **fresh context that receives only its brief**. If you cannot spawn isolated subagents here, stop and say so — one agent role-playing five defeats the point and is worse than implementing the plan honestly.

The plan already exists when this skill runs. Note where it lives; it's the input to three of the five roles.

## Ground rules

- **Only the implementer, test-writer, and mutation tester modify files.** Critic and plan-verifier are read-only; their findings come back through you and get routed to whoever owns the fix.
- **You run the test suite, linter, and typechecker** — not the agents. Five agents running the same suite is five copies of the same output burning context.
- **Only pre-existing tooling.** Do not introduce test frameworks, linters, or mutation libraries.
- **No commits.** The working tree is the diff. Build briefs from `git diff` plus untracked files.
- **Keep a findings ledger in a file**, wherever suits this environment. It survives context compaction, which an in-context ledger doesn't — and once the ledger is summarised away, resolved nits come back from the dead and repeat-detection silently stops working. **Delete it when the run ends.**
- **Run to completion autonomously.** Nothing here stops to ask the user a question; surface it in the final report instead.

## The roles

Prompts live in `roles/`. Read one at a time, as you dispatch it.

| Role | File | Writes | Brief contains | Must never see |
|---|---|---|---|---|
| Implementer | `roles/implementer.md` | ✅ | Plan, repo access, its own findings-to-fix | Tests, critic reasoning, other briefs |
| Test-writer | `roles/test-writer.md` | ✅ | Plan, repo access, impl signatures (collisions only) | Implementation bodies |
| Critic | `roles/critic.md` | ❌ | Diff + test diff | The plan, round history |
| Mutation tester | `roles/mutation-tester.md` | ✅ (temporarily) | Diff + tests + how to run the suite | The plan, critic findings |
| Plan-verifier | `roles/plan-verifier.md` | ❌ | Plan + diff + file tree | Test internals, critic findings |

Two of these will feel wrong and are load-bearing:

**The test-writer never sees implementation bodies.** Tests written against code describe what the code does, bugs included. Tests written against the plan describe what it *should* do. When they disagree, that disagreement is the signal you're paying for.

**The critic never sees the plan.** Its question is whether this code and these tests are any good on their own merits. Give it the plan and it re-litigates scope, overlaps with the plan-verifier, and can be talked out of a real complaint with "well, the plan said so".

## Pipeline

One pass over the whole plan. If the plan declares phases, treat each phase as its own pass.

### 1. Build in parallel

Dispatch the implementer and test-writer **at the same time**, both from the plan, neither aware of the other. They will disagree about interfaces — that's the point.

### 2. Green gate

Run the suite, plus linter and typechecker if already configured. Red never reaches the critics.

**You adjudicate failures, against the plan:**
- Plan specifies the interface → whoever deviated fixes it.
- Plan is silent → **the implementation is authoritative.** Send the test-writer the public surface — signatures only — to re-align. Tests shouldn't dictate design nobody agreed to.

### 3. Critic

Dispatch a **fresh** critic over the diff and test diff. Fresh every round: isolation is the priority and you are the memory. It classifies each finding:

- **blocker** / **defect** — routed to the owner; the loop continues until they're gone.
- **nit** — reported once, recorded, never re-raised. Strip already-logged nits from each new report before acting, or a fresh critic rediscovers the same cosmetic preferences forever and the loop never ends.

Route fixes as a new brief containing only the finding. The critic never fixes what it finds — that keeps it honest and stops two agents editing the same files at once.

### 4. Mutation tester

Runs **strictly alone** — builds and simulators don't share well, and a half-mutated tree read by anything else produces garbage.

**Snapshot the working-tree diff before dispatching.** Verify the tree matches it afterwards and restore it yourself if not. With no commits, that's the only safety net for an agent that dies mid-mutation.

It reports every surviving mutant, including trivial-looking ones, and argues its case. **It is deliberately uncompromising and you adjudicate** — a mutation tester allowed to excuse itself stops catching anything, but a loop chasing every survivor never ends. Its one self-dismissal is **equivalent** (genuinely can't change behaviour, so unkillable), which is a fact rather than a judgement.

Route the rest: weak tests → test-writer; dead or unreachable code → implementer. A survivor you rule not worth a test goes in the ledger **with the tester's argument for it**, so what got waved off stays visible.

### 5. Loop

Return to the green gate. Exit when, in the same round, **the critic returns zero blockers and defects, and every survivor is killed, equivalent, or accepted by you**.

**Termination guard:** when the same unresolved finding appears a third time, rule it won't-fix, record it with what was attempted, and carry on. Keeps the run autonomous while still guaranteeing it ends — and the ruling lands in the report with its history, so the user can overrule you.

### 6. Plan-verifier

Dispatch over the plan, diff, and file tree. Relay each gap to the implementer, which answers *"missed it"* (re-enters the loop) or *"deliberate, because X"* (record the justification).

### 7. Clean up

Delete the ledger file.

## Final report

Short and fixed. The user has the diff; don't narrate it back.

```
## Built
<one or two lines>

## Rounds
<count>

## Deliberate deviations from the plan
<item — implementer's justification>

## Reported, not fixed
<nits, and findings ruled won't-fix with what was attempted>

## Surviving mutants accepted
<mutant — the tester's argument, and why you accepted it anyway>
```

Empty sections are good news, but say "none" rather than dropping the heading — the user should be able to tell "nothing to report" from "forgot to check".
