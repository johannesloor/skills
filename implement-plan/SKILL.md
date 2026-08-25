---
name: implement-plan
description: Execute an approved plan through a gauntlet of isolated subagents — specifier, implementer, test-writer, cleaner, hardener, QA, and plan-verifier — each seeing only what its own job requires. Use this whenever a plan has been agreed and it is time to write the code, such as after plan mode is approved, after a spec or issue is settled, or when the user says "implement this", "build the plan", "go ahead", or "start coding". Prefer this over implementing a plan directly yourself, even for plans that look small.
---

# Implement a plan

You are the **coordinator**. You do not write code. You run seven specialist roles, each in a fresh context that receives only its own brief, and you are the sole channel between them.

The idea is a gauntlet: code that survives every stage can be trusted without anyone reading every line of it. That only works if the stages are genuinely independent. An agent that implements, tests, and reviews its own work grades its own homework — it writes tests that pass because it already knows what the code does. Agents that never see each other's reasoning can't collude. So be disciplined about what you hand each one: every extra file in a brief is a chance for one role to inherit another's assumptions.

## Prerequisite

Each role must run in a **fresh context that receives only its brief**. If you cannot spawn isolated subagents here, stop and say so — one agent role-playing seven defeats the point and is worse than implementing the plan honestly.

The plan already exists when this skill runs. Note where it lives; it's the input to the specifier.

## Ground rules

- **Only the implementer, test-writer, hardener, and QA agent modify files.** Cleaner and plan-verifier are read-only; their findings come back through you and get routed to whoever owns the fix.
- **The specifier's artifacts are frozen.** No downstream agent may edit the acceptance criteria or QA procedure. An implementer that can soften its own acceptance criteria has no acceptance criteria. If a criterion is genuinely wrong, *you* change it — and say so in the final report, so a moved goalpost is never invisible.
- **You run the test suite, linter, and typechecker** — not the agents. Seven agents running the same suite is seven copies of the same output burning context.
- **Only pre-existing tooling.** Do not introduce test frameworks, linters, or mutation libraries.
- **No commits.** The working tree is the diff. Build briefs from `git diff` plus untracked files.
- **Keep a findings ledger in a file**, wherever suits this environment. It survives context compaction, which an in-context ledger doesn't — and once the ledger is summarised away, resolved nits come back from the dead and repeat-detection silently stops working. **Delete it when the run ends.**
- **Run to completion autonomously.** Nothing here stops to ask the user a question; surface it in the final report instead.

## The roles

Prompts live in `roles/`. Read one at a time, as you dispatch it.

| Role | File | Writes | Brief contains | Must never see |
|---|---|---|---|---|
| Specifier | `roles/specifier.md` | ✅ criteria only | Plan, repo (read-only) | — (runs before code exists) |
| Implementer | `roles/implementer.md` | ✅ | Plan, criteria, repo, its findings-to-fix | Tests, reviewer reasoning |
| Test-writer | `roles/test-writer.md` | ✅ | Plan, criteria, repo, impl signatures (collisions only) | Implementation bodies |
| Cleaner | `roles/cleaner.md` | ❌ | Diff + test diff | The plan, round history |
| Hardener | `roles/hardener.md` | ✅ (temporarily) | Diff + tests + how to run the suite | The plan, cleaner findings |
| QA | `roles/qa.md` | ✅ script only | QA procedure + how to run the system | The plan, the diff, all source |
| Plan-verifier | `roles/plan-verifier.md` | ❌ | Plan + criteria + diff + file tree | Test internals, cleaner findings |

Three of these will feel wrong and are load-bearing:

**The test-writer never sees implementation bodies.** Tests written against code describe what the code does, bugs included. Tests written against the plan describe what it *should* do. When they disagree, that disagreement is the signal you're paying for.

**The cleaner never sees the plan.** Its question is whether this code and these tests are any good on their own merits. Give it the plan and it re-litigates scope, overlaps with the plan-verifier, and can be talked out of a real complaint with "well, the plan said so".

**The QA agent never sees any source.** It knows only what a user should be able to do. An agent that can't see the implementation can't be led by it — and it's the only stage that checks what the user actually receives rather than a description of it.

## Pipeline

One pass over the whole plan. If the plan declares phases, treat each phase as its own pass.

### 1. Specify

Dispatch the specifier over the plan. It produces **acceptance criteria** (concrete, checkable behaviour) and a **QA procedure** (how a human would exercise the running system). Both are frozen from this moment.

This runs before any code exists so nothing can drift toward what was built. If the specifier reports the plan is too vague to make concrete, that's the cheapest possible moment to find out — surface it and use your judgement.

### 2. Build in parallel

Dispatch the implementer and test-writer **at the same time**, both from the plan and criteria, neither aware of the other. They will disagree about interfaces — that's the point.

### 3. Green gate

Run the suite, plus linter and typechecker if already configured. Red never reaches the cleaner.

**You adjudicate failures, against the plan and criteria:**
- Plan or criteria specify the interface → whoever deviated fixes it.
- Both are silent → **the implementation is authoritative.** Send the test-writer the public surface — signatures only — to re-align. Tests shouldn't dictate design nobody agreed to.

### 4. Cleaner

Dispatch a **fresh** cleaner over the diff and test diff. Fresh every round: isolation is the priority and you are the memory. It classifies each finding:

- **blocker** / **defect** — routed to the owner; the loop continues until they're gone.
- **nit** — reported once, recorded, never re-raised. Strip already-logged nits from each new report before acting, or a fresh cleaner rediscovers the same cosmetic preferences forever and the loop never ends.

Route fixes as a new brief containing only the finding. The cleaner never fixes what it finds — that keeps it honest and stops two agents editing the same files at once.

### 5. Hardener

Runs **strictly alone** — builds and simulators don't share well, and a half-mutated tree read by anything else produces garbage.

**Snapshot the working-tree diff before dispatching.** Verify the tree matches it afterwards and restore it yourself if not. With no commits, that's the only safety net for an agent that dies mid-mutation.

It reports every surviving mutant, including trivial-looking ones, and argues its case. **It is deliberately merciless and you adjudicate** — a hardener allowed to excuse itself stops catching anything, but a loop chasing every survivor never ends. Its one self-dismissal is **equivalent** (genuinely can't change behaviour, so unkillable), which is a fact rather than a judgement.

Route the rest: weak tests → test-writer; dead or unreachable code → implementer. A survivor you rule not worth a test goes in the ledger **with the hardener's argument for it**, so what got waved off stays visible.

### 6. Loop

Return to the green gate. Exit when, in the same round, **the cleaner returns zero blockers and defects, and every survivor is killed, equivalent, or accepted by you**.

**Termination guard:** when the same unresolved finding appears a third time, rule it won't-fix, record it with what was attempted, and carry on. Keeps the run autonomous while still guaranteeing it ends — and the ruling lands in the report with its history, so the user can overrule you.

### 7. QA

Once the loop is clean, dispatch the QA agent against the **real running system** with the frozen QA procedure. Skip only if the host genuinely cannot run the system — and say so in the report, because a visibly skipped check is fine while a silently absent one is not.

Prefer the executable route (it re-runs identically); fall back to driving the app live. **Label which you got** — executable results are *verified*, live interaction is *observed*, and they are not the same strength of claim.

A QA failure re-opens the loop: fix, then green gate → cleaner → hardener again before re-running QA. That's expensive, which is exactly why QA runs late.

### 8. Plan-verifier

Dispatch over the plan, criteria, diff, and file tree. Relay each gap to the implementer, which answers *"missed it"* (re-enters the loop) or *"deliberate, because X"* (record the justification).

### 9. Clean up

Delete the ledger file.

## Final report

Short and fixed. The user has the diff; don't narrate it back.

```
## Built
<one or two lines>

## Rounds
<count>

## QA
<verified by script | observed interactively | skipped, because ...>

## Deliberate deviations from the plan
<item — implementer's justification>

## Criteria changed mid-run
<criterion — what it was, what you changed it to, why>

## Reported, not fixed
<nits, and findings ruled won't-fix with what was attempted>

## Surviving mutants accepted
<mutant — the hardener's argument, and why you accepted it anyway>
```

Empty sections are good news, but say "none" rather than dropping the heading — the user should be able to tell "nothing to report" from "forgot to check".
