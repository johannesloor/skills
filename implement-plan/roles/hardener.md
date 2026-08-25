# Role: Hardener

Writes files, temporarily. Runs **strictly alone** — nothing else may touch the repository while it works.

## Brief template

```
Test whether this change's tests would actually catch a bug, by introducing bugs
on purpose.

Changed files:
<list, or the diff>

Test files:
<list>

Run the suite with: <command>

Method:
1. Confirm the suite passes before you touch anything. If it doesn't, stop and
   report that — mutation results mean nothing against a red suite.
2. Pick mutants that target the logic this change introduced: flip a condition,
   change a boundary (< to <=), swap an operator, return a wrong-but-plausible
   value, skip a side effect, remove an early return, drop an error case.
3. Apply ONE mutant at a time. Run the suite. Record whether it failed (mutant
   killed) or passed (mutant survived). Revert that mutant before the next.
4. Confirm at the end that every mutant is reverted and the tree is exactly as
   you found it.

Aim for meaningful coverage of the changed logic rather than a fixed count —
enough mutants that a surviving one tells you something real.

For each SURVIVING mutant report: file, line, the exact change you made, your read
on why nothing caught it, and the argument for why it matters.

Report every survivor. Do not filter for what you assume is worth the effort, and
do not talk yourself out of one because the surface looks unimportant — a log line
nobody tests is exactly where a silent behaviour change lives. Someone else weighs
cost against value; your job is to make sure nothing goes unreported.

The one exception is an EQUIVALENT mutant: one where your change genuinely does not
alter behaviour, so no test could ever kill it. That's a fact about the code, not a
judgement about effort, so call it out as equivalent and explain why.

Killed mutants need only a count. Do not modify tests, do not fix anything, and
do not commit.
```

## Notes for the coordinator

- Do not dispatch this against a red suite; the results are meaningless.
- The tester's read on *why* a mutant survived is a hypothesis, not a verdict — you decide the routing.
- **Equivalent** needs no ruling from you: record it and move on.
