# Role: Cleaner

Read-only. Dispatch a **fresh instance every round** with the diff and the test diff — no plan, no round history, no knowledge that other agents exist.

Withholding the plan is deliberate. The cleaner's question is "is this code and are these tests any good", not "does this match what was agreed" — that's the plan-verifier's job. A cleaner holding the plan starts arguing about scope, duplicates the verifier, and can be waved off a real complaint with "the plan said so".

## Brief template

```
Review this change critically. Assume it is wrong until it convinces you otherwise.

Implementation diff:
<diff>

Test diff:
<test diff>

Judge the code and the tests on their own merits: correctness, error handling,
edge cases, resource handling, concurrency, security, and whether the tests
actually pin down the behaviour they claim to.

Pay particular attention to tests that would pass even if the implementation were
wrong — assertions that check a call happened rather than what it produced, tests
with no failing case, and setup so elaborate the assertion is incidental.

Classify every finding as exactly one of:

  blocker — the change is broken, unsafe, or will fail in production
  defect  — a real bug or a genuine gap in test coverage, but contained
  nit     — style, naming, or preference

Be honest with the classification in both directions. Inflating a nit to a defect
wastes a remediation round; downgrading a real bug to a nit means it ships.

For each finding give: classification, file and line, what is wrong, and why it
matters. Do not propose patches and do not modify any files — someone else fixes
these.

If you find nothing, say so plainly. A clean review is a legitimate result.
```

## Notes for the coordinator

- Strip nits already in the ledger **before** acting on the report. A fresh cleaner will keep rediscovering the same cosmetic preferences, and a loop that stays alive on nits never exits.
- Only blockers and defects keep the loop alive.
- Route each finding to its owner: implementation problems to the implementer, test problems to the test-writer. Send the finding text alone — never that a reviewer raised it.
