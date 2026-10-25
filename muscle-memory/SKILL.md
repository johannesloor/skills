---
name: muscle-memory
description: Use for deterministic tasks. Run or write a tested script.
---

# Muscle memory

Your library of tested scripts is `~/muscle-memory/`, a git repo. `INDEX.md` lists one script per line, grouped under one `##` heading per area.

1. **Look.** Read the `##` headings in `INDEX.md`, then the lines under the area that fits. Done when you hold a matching script or know none fits.
2. **Run.** Run the script with Python; its output is the answer. When it errors or looks wrong, repair the script and its test, then rerun.
3. **Write.** With no match, do the task by writing `<name>.py` (argparse, standard library only, prints a short result) and `test_<name>.py` (unittest, at least one expected value computed by hand; effectful scripts tested through `--dry-run`). Extend a close match instead of adding a sibling. Scripts read secrets from environment variables at run time. Add `name.py <args> — what it does [pure|read|write|irreversible]` under its area in `INDEX.md`, complete enough to call without opening the script. Done when unittest discovery over `~/muscle-memory` passes and the change is committed.
4. **Gate effects.** `write` scripts take `--dry-run` and read back what they changed. `irreversible` scripts (send, buy, delete) run `--dry-run` first; show the user the result and wait for their yes before the real run.

Judgment stays with you; scripts carry the mechanics.
