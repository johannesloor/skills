---
name: muscle-memory
description: Use for deterministic tasks. Run or write a tested script.
---

# Muscle memory

A deterministic task has one right answer every run (compute, convert, count, check, edit a file or calendar), so it belongs in a script and you are its caller. The library is `~/muscle-memory/` (a git repo); `INDEX.md` lists one script per line.

1. **Look.** Read `INDEX.md`. Done when you hold a matching script or know none fits.
2. **Run.** `python3 ~/muscle-memory/<name>.py <args>`; its output is the answer. When it errors or looks wrong, repair the script and its test, then rerun.
3. **Write.** With no match, do the task by writing `~/muscle-memory/<name>.py` (argparse, stdlib, prints a short result) and `test_<name>.py` (unittest, at least one expected value computed by hand; effectful scripts tested through `--dry-run`). Extend a close match instead of adding a sibling. Secrets come from Keychain at run time: `security find-generic-password -s muscle-memory/<key> -w`. Add `name.py <args> — what it does [pure|read|write|irreversible]` to `INDEX.md`, complete enough to call the script without opening it. Done when `python3 -m unittest discover -s ~/muscle-memory` passes and the change is committed.
4. **Gate effects.** `write` scripts take `--dry-run` and read back what they changed. `irreversible` scripts (send, buy, delete) run `--dry-run` first; show the user the result and wait for their yes in chat before the real run.

Judgment stays with you; scripts carry the mechanics.
