---
name: review
description: Review code changes against their intent and surrounding code. Use after implementation or when asked to review a diff, branch, or PR. Read-only: reports findings and never modifies or publishes anything.
---

Review the complete change and stop with findings.

1. Establish intent from the user request, approved plan, PR description, and applicable repository instructions. Check that the diff delivers the Goal and Finish line and stays out of Excluded. If missing context would materially change the review, ask one question before continuing.

2. Determine the correct base. For stacked work, use the parent branch rather than the repository default. Include committed, staged, unstaged, and relevant untracked changes.

3. Read every changed file and only the surrounding code needed to understand it: callers, the readers of any new output, contracts, tests, and established patterns. For every changed shared function, constant, threshold or matching rule, find all its callers in the repo, including alerts, APIs and jobs, and report any whose behavior changed unintentionally. Test a changed rule on inputs beyond the examples it was tuned on, including the cases the previous behavior protected, and report a failing class, not one case. Check that each new test would fail on the old code. On a re-review, recheck every earlier finding, not only the new lines.

4. Report only substantiated issues introduced or exposed by the change. Each finding needs a location, concrete failure or violated invariant, impact, and the smallest reasonable fix direction. Judge correctness, failure handling, framework conventions, and test assertions against the agreed behavior and local patterns. Flag unnecessary machinery introduced by the change: for each feature, field, query clause, or branch, ask what breaks if it's removed. If nothing agreed breaks, it's a finding; its own test doesn't count. Standard practice and typical edge cases count as agreed; paranoid ones don't. Check structure only where the diff adds it. New or newly written shared state (global, cache, shared row): name every writer and whether one place fixes their order. New logic: does it read the DB, clock, env or a global it could take as an argument, or change an argument its caller still uses? New class, base class, interface or layer: what are its two uses today, or what rule on its own data does it guard? New helper: grep for existing code that does the same and cite it. Report these only with a concrete cost: a reachable bug, or code that can be deleted with the agreed behavior intact. Don't recommend speculative redesign. Explain what can be removed and why the agreed behavior still holds. Skip pre-existing issues, style or linter findings, and vague requests for more tests.

5. Return the reviewed scope, understood intent, severity-ordered findings, genuine questions, and anything you could not review. If there are no findings, say so plainly.

Never edit files, run verification, change Git state, post comments, approve, resolve threads, or fix findings. Do not rerun unless the user asks.
