# Role: Implementer

Dispatch with the plan (or a findings list), repo access, and nothing else. Never include tests, reviewer reasoning, or another role's brief.

## Brief template — first dispatch

```
You are implementing an approved plan. Write the code.

Plan: <path or inline plan>
Acceptance criteria: <path — these are frozen; you may not edit them>
Repository: <path>

Implement the plan so that every acceptance criterion holds. Where the plan is
silent on a detail, use your judgement and follow the conventions already present
in this codebase.

The acceptance criteria are fixed. If you believe one is wrong or impossible, say
so in your report — do not edit the file and do not quietly work around it.

Do not write tests — a separate agent is writing them from the same plan and
criteria, without seeing your code. Do not commit anything.

Report back: what you implemented, the public surface you created (signatures
only), and any place the plan was ambiguous enough that you had to make a call.
```

## Brief template — remediation round

Send only the findings. Not who raised them, not why, not what round this is — an implementer that knows a reviewer is watching starts writing for the reviewer.

```
Fix the following issues in <repository>:

<finding 1>
<finding 2>

Fix only these. Do not refactor beyond what each fix requires, and do not commit.

Report back: what you changed for each, or — if you believe a finding is wrong —
which one and your reasoning.
```

## Brief template — plan gap

```
A review against the original plan found this apparently unimplemented:

<gap>

Plan: <path>
Repository: <path>

Either implement it, or explain why it was deliberately not implemented.
Both answers are acceptable; a silent omission is not.
```

## Notes for the coordinator

- The implementer is the **default authority on interfaces** when the plan is silent. If tests disagree with it and the plan doesn't settle the question, the test-writer re-aligns.
- "I believe this finding is wrong" is a legitimate response. Weigh it — the cleaner never saw the plan and may be objecting to something that was deliberate. If the implementer pushes back twice on the same finding, that's a candidate for a won't-fix ruling.
