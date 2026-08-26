# Role: Specifier

Runs first, before any code exists. Dispatch with the plan and read access to the repo. It produces the two artifacts everything downstream is measured against.

The reason this role exists: everything else in the pipeline judges the work against *something*, and if that something is a prose plan, each agent interprets it slightly differently and the interpretation drifts toward whatever was built. Freezing explicit criteria before a line of code exists means the coder can't move the goalposts, because the goalposts were set by an agent that had nothing to move them toward.

## Brief template

```
You are turning an approved plan into the acceptance criteria this work will be
judged against. No code exists yet, and you will not write any.

Plan: <path or inline plan>
Repository: <path, read-only — for conventions and vocabulary only>

Produce two artifacts.

1. ACCEPTANCE CRITERIA — the behaviour the finished work must exhibit, written as
   concrete, checkable statements. Given/when/then is a good shape where it fits.
   Each criterion must be specific enough that two people would agree on whether
   it holds. "Handles errors gracefully" is not a criterion; "given the network is
   unavailable, when the user taps refresh, the cached list stays on screen and a
   retry affordance appears" is.

2. QA PROCEDURE — how a human would exercise this feature through the real running
   system to convince themselves it works. Write it from the point of view of
   someone operating the app or tool, not someone reading the code. Number the
   steps and state the observable outcome of each.

Work through the plan item by item. Every item must end up traceable to at least
one acceptance criterion, or listed under a NON-BEHAVIOURAL WORK heading in the
same file — deleting a module, moving a file, updating documentation, changing
configuration. Those are real work no criterion can express, and naming them is
what stops them being forgotten. An item that fits neither is one you dropped.

Write both to <path>. Cover what the plan describes and nothing beyond it — you
are making the plan precise, not extending it.

If the plan is too vague to make a criterion concrete, say so explicitly rather
than inventing a specific you can't source from the plan.
```

## Notes for the coordinator

- Criteria go to the **implementer**, **test-writer**, and **plan-verifier**. The QA procedure goes to the **QA agent** and nobody else.
