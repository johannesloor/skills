---
name: muscle-memory
description: Use for deterministic tasks. Run or write a tested script.
---

# Muscle memory

Your library of tested scripts lives at `$MUSCLE_MEMORY_DIR`, or `~/.muscle-memory/` when that is unset. It is a git repo whose `INDEX.md` opens with the library's language, its run command and one test command that runs every test, both working from any directory, then lists one script per line under one `##` heading per area. With no library yet, create it in a language the environment already runs, and ask the user whether it stays local or pushes to a remote (suggest private); record the answer as `Remote:` in the header.

1. **Look.** Read the `##` headings in `INDEX.md`, then the lines under the area that fits. Done when you hold a matching script or know none fits.
2. **Run.** Run the script with the library's run command; its output is the answer. Done when it exits cleanly and its output answers the task; otherwise repair the script and its test, then rerun.
3. **Answer, then write.** With no match, give the user the result first, by the quickest reliable means. Then, if the task's general form is likely to recur, write a script for that general form, in a background subagent where you have one. Write it in the library's language (command-line arguments, standard library only, prints a short result) and its test (at least one expected value computed by hand; effectful scripts tested through `--dry-run`). Extend a close match when one exists. Scripts read secrets from environment variables at run time. Add `<script> <args> — what it does [pure|read|write|irreversible]` under its area in `INDEX.md`, complete enough to call without opening the script. Done when the user has the result and any new script reproduces it (or the user has the correction), passes the library's test command, and is committed, and pushed when `Remote:` names one.
4. **Gate effects.** `write` scripts take `--dry-run` and read back what they changed. `irreversible` scripts (send, buy, delete) run `--dry-run` first; show the user the result and wait for their yes before the real run.
