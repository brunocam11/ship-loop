---
name: next-task
description: Invoked as `/next-task <ledger>` in a fresh conversation in the repo. Builds the next unchecked slice from a feature's stack ledger: reconcile against git, pick the right base (stack on an open reviewed PR when the dep is not merged yet), implement vertically with TDD, and stop before merge. Run after manually opening a new pane + claude.
---

Run this in a fresh conversation, in the target repo, to build one slice of a `slice` ledger. You implement only this slice and stop before merge. Review and merge stay with the user.

Args: `<ledger>` (required; the stack ledger path is how this fresh conversation gets the plan, e.g. `.claude/timeline-provenance.md`) and optional `[PR#]` (defaults to the next unchecked slice whose deps are done, merged or in an open reviewed PR).

1. Read the passed ledger. If it was omitted, find the one `.claude/*.md` with open `- [ ]` slices; if several, list them and ask.
2. Reconcile against git. The ledger is only a cursor: check `gh pr list --state all` / `git log` to see what actually merged or is open, trust git over the checkboxes, and fix the cursor if it drifted.
3. Pick the target slice (the given PR#, else the first unchecked slice whose deps are done) and pick its base branch:
   - dep merged to develop: branch off develop.
   - dep still in an open, greplooped PR: branch off that PR's branch (stacked), so you do not wait for the merge. Record it in the ledger as stacked on that PR; it needs a restack onto develop when the dep merges.
   - dep not built or PR'd yet: stop and say which.
4. Build only that slice, on that branch:
   - Make a todo list for it.
   - TDD where unit-testable: failing test first, then implement. UI/glue: a one-line "does X, breaks on Y" contract.
   - Vertical: every layer this slice touches, nothing from later slices.
   - Run the project's verify command (lint, types, unit tests — e.g. `make verify` or `npm run verify`) and the integration suite if the slice hits it, before handing back.
5. Do NOT merge. Hand back for review, then `/submit`. When the dep PR later merges, restack this branch onto develop. After this slice merges, tick it in the ledger, move the cursor, and link the PR number.
