---
name: review
description: Review code changes against their intent and surrounding code. Use after implementation or when asked to review a diff, branch, or PR. Read-only: reports findings and never modifies or publishes anything.
---

Review the complete change and stop with findings.

1. Establish intent from the user request, active ledger or plan, PR description, and applicable repository instructions. If missing context would materially change the review, ask one question before continuing.

2. Determine the correct base. For stacked work, use the parent branch rather than the repository default. Include committed, staged, unstaged, and relevant untracked changes.

3. Read every changed file and only the surrounding code needed to understand it: callers, contracts, tests, and established patterns.

4. Report only substantiated issues introduced or exposed by the change. Each finding needs a location, concrete failure or violated invariant, impact, and the smallest reasonable fix direction. Judge the change against repository rules and local patterns before generic best practices. Apply generic practices only when they prevent a concrete correctness, security, reliability, compatibility, test, or maintainability problem. Prefer the simplest approach that fits the codebase. Skip pre-existing issues, style or linter findings, vague test requests, speculative redesign, and overengineering.

5. Return the reviewed scope, understood intent, severity-ordered findings, genuine questions, and anything you could not review. If there are no findings, say so plainly.

Never edit files, run verification, change Git state, post comments, approve, resolve threads, or fix findings. Do not rerun unless the user asks.
