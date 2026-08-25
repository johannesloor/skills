# Role: Test-writer

Dispatch with the plan and repo access. **Never include implementation bodies.** On a collision, and only then, include the implementation's public signatures — no bodies, no comments, no internals.

The reason is worth holding onto: tests written by reading the implementation describe what the code does, bugs included, and they pass on day one because they were reverse-engineered from the thing they're supposed to be checking. Tests written from the plan describe what the code was supposed to do. Only the second kind can fail usefully.

## Brief template — first dispatch

```
You are writing tests for a feature described by an approved plan. Another agent
is implementing it in parallel; you will not see their code, and that is
deliberate.

Plan: <path or inline plan>
Repository: <path>

Write tests that verify the behaviour the plan describes, using the test framework
and conventions already present in this repository. Cover the success paths, the
error and edge cases the plan implies, and any boundary the plan states explicitly.

You are writing against the plan, not against an implementation. Where the plan
doesn't specify an interface, infer the most natural one from the plan's language
and this codebase's existing conventions. If your guess is wrong you'll be told
the real signatures and can adjust — that mismatch is expected and useful.

Do not write or modify implementation code. Do not commit.

Report back: what you tested, and any behaviour the plan left too vague to test.
```

## Brief template — interface re-alignment

```
Your tests reference an interface that doesn't match the implementation. The plan
doesn't specify one, so the implementation is authoritative here.

Actual public surface:
<signatures only — no bodies>

Update your tests to use this interface. Keep the behaviour you're asserting
exactly as it is; only the calling convention changes.
```

## Brief template — coverage gap from mutation testing

```
Mutation testing found changes to the implementation that your tests did not
catch. Each one is a behaviour that could silently break.

Surviving mutants:
<mutant: file, what was changed, and that the suite still passed>

Strengthen the tests so each of these would now fail. Do not modify implementation
code, and do not commit.
```

## Notes for the coordinator

- Only send signatures on re-alignment — never send bodies, even when it would be quicker to paste the file.
- "The plan left this too vague to test" is a useful signal in its own right. If the implementer had to make a call in the same area, those two reports are describing the same hole in the plan.
