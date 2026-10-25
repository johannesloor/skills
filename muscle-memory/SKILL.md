---
name: muscle-memory
description: Use before computing, parsing or converting by hand, or writing a snippet twice. Run or write a tested script.
---

# Muscle memory

Your library of tested scripts lives at `$MUSCLE_MEMORY_DIR`, or `~/.muscle-memory/` when that is unset. It is a git repo whose `INDEX.md` opens with the library's language, its run command and one test command that runs every test, both working from any directory, then lists one script per line under one `##` heading per area. With no library yet, create it in a language the environment already runs, and ask the user whether it stays local or pushes to a remote (suggest private); if a remote, add it with `git remote add`.

Check at each sub-step of a larger task, not only at its start.

1. **Look.** Read the `##` headings in `INDEX.md`, then the lines under the area that fits. Done when you hold a matching script or know none fits.
2. **Run.** Run the script with the library's run command. Done when it exits cleanly; its output goes to the user as the answer. On an error, repair the script and its test, then rerun.
3. **Answer, then write.** With no match, give the user the result before writing any script. Then, if the task's general form is likely to recur, write a script for it (command-line arguments, standard library only, prints a short result) and its test (at least one expected value computed by hand), in a background subagent where you have one. Extend a close match when one exists. Scripts read secrets from environment variables at run time. Add `<script> <args> — what it does [pure|read|write|irreversible]` under its area in `INDEX.md`, complete enough to call without opening the script. Done when the new script reproduces the result you gave, passes the library's test command, and is committed (and pushed, when `git remote` lists one). When the two disagree, find which is wrong: fix the script, or send the user the corrected result.
4. **Gate effects.** `write` and `irreversible` scripts take `--dry-run`, which their tests use. `write` scripts read back what they changed. `irreversible` ones (send, buy, delete) dry-run first; show the user the dry-run output and wait for their yes before the real run.
