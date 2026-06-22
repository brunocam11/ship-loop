# Playbook

Skills are verbs. Compose only what the task needs; nothing forces a stage order. Start where the task starts.

Build a verb only when a real task needs it.

## Verbs
- `scope` — ambiguous ask -> defined task (problem statement + out-of-scope + options or one task, with the contract). The entry point when the task isn't defined yet.
- `grill-me` — lock the design; resolve every branch before building. The default start for defined, non-trivial work.
- `slice` — approved plan -> ordered stack of small, independently-reviewable PRs, written in place as the feature's progress ledger (`.claude/<feature>.md`, local). Size-gate first: skip slicing if it's one PR (~800-1k LOC incl. tests).
- `next-task` — in a fresh conversation in the repo, `/next-task <ledger>` builds the next/given slice from the ledger: reconcile vs git, vertical TDD, stop before merge. The execution step for each `slice` increment.
- `submit` — commit the reviewed slice (one-line conventional commit, never any attribution) and open its PR with a concise reviewer-oriented body. Runs after `/next-task` and your review.
- `greploop` — iterate the PR to a clean code-review score, zero unresolved.

## Recipes
Start where the task starts: ambiguous -> `scope`; defined task -> `grill-me`.
- Feature: `(scope if fuzzy) -> grill-me -> slice -> [failing test -> implement] per slice -> greploop -> ship`.
- Bugfix: reproduce `-> failing test -> implement -> greploop`.
- Refactor: read code as truth `-> characterization test -> implement`.
- Big cross-cutting feature (back + front + DB + deploy): `scope -> grill-me (lock the contract: API shape / schema / shared types) -> land the foundation first (migration via expand/contract + contract types, its own small PR) -> parallel worktrees for independent seams (backend, frontend, each against the contract) + stacked PRs for the dependent chain -> integrate behind a feature flag -> verify end-to-end -> ship the vertical slice -> cleanup PR`.

## Operating rules
- Grill to start: any defined non-trivial task begins with `grill-me` — decide the design before building, so you understand it before it exists.
- Contract-first decomposition: before splitting a big feature, lock the contract (API shape, schema change, shared types). The contract is the source of truth that makes layers independent. Parallelize only non-overlapping seams (separate git worktrees, one session each); express dependent chains as stacked PRs. Keep every step backward-compatible and behind a flag so it stays reversible.
- TDD: tests come first and are written by the workflow, not by hand. Red (failing test) -> green (implement) -> refactor. You own them by reviewing, not authoring.
- Model routing: best model for scope/grill/architecture/review; cheaper/faster for mechanical implementation.
- Read real source: touching an unfamiliar dependency, read its real source first (e.g. with `opensrc`), don't guess.
- Self-review before human: run `greploop` and explain the load-bearing part back before opening for review.
- Explore via subagent: broad codebase or library sweeps go to a subagent that returns the conclusion, keeping main context lean.
