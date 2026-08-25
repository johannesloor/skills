# Role: QA

Runs once, after the hardener, against the **real running system**. Dispatch with the QA procedure and how to launch the system — nothing else.

This is the purest isolation in the pipeline. The QA agent has never seen the plan, the diff, or a line of source. It cannot be led by the implementation because it doesn't know what the implementation looks like — it only knows what a user is supposed to be able to do. It's also the only stage that checks the thing the user actually receives rather than a description of it.

## Prerequisite

The host must be able to run the system: a simulator or device, a dev server and browser, an executable, whatever this project produces. If it can't, skip this stage and say so in the final report. A visibly skipped check is fine; a check that silently never happened is not.

## Brief template — executable

Preferred whenever the system can be driven programmatically — an existing UI test target, a CLI, HTTP endpoints.

```
Verify a feature works by exercising the real running system. You have not seen
the source code and do not need it.

QA procedure:
<procedure>

How to run the system: <command / scheme / URL>
How to drive it: <existing UI test target, CLI, endpoints — whatever exists here>

Write a script that performs the QA procedure against the running system and
produces a deterministic pass or fail for each step. Use the tooling already
present in this project; do not introduce a new framework.

Run it. Report each step as passed or failed, with the observed result for any
failure.

If a step cannot be automated with what's available here, say which one and why
rather than approximating it.
```

## Brief template — interactive fallback

When the only route is driving the app live.

```
Verify a feature works by operating the real running system, as a user would.
You have not seen the source code and do not need it.

QA procedure:
<procedure>

How to run the system: <command / scheme / how to launch>
How to interact: <the tools available in this environment>

Work through the procedure step by step. For each step, report what you did and
what you actually observed — not what you expected.

Be literal about observations. "The screen looked right" is not a result; "the
list showed 3 items and the header read 'Downloads'" is.

If you cannot reach a step — the app won't launch, a control isn't there — report
that as a blocked step rather than working around it.
```

## Notes for the coordinator

- **Label the evidence honestly in the final report.** Executable runs are *verified*; interactive runs are *observed*. They are not the same strength of claim, and the difference matters when someone later asks what was actually checked.
- Failures route to the **implementer**, with the failing step and the observed result only — never the source, never who reported it.
- A QA failure re-opens the loop: fix, then re-run the green gate, cleaner, and hardener before QA again. Expensive, which is why QA runs last.
- "This step couldn't be automated" and "this step was blocked" are findings, not excuses. Carry them into the report; a procedure nobody can execute is a design problem worth knowing about.
