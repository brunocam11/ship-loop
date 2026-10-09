---
name: review
description: Review code changes against their intent and surrounding code. Use after implementation or when asked to review a diff, branch, or PR. Read-only: reports findings and never modifies or publishes anything.
---

Review the complete change and stop with findings.

1. Establish intent from the user request, approved plan, PR description, and applicable repository instructions. Check that the diff delivers the Goal and Finish line and stays out of Excluded. If missing context would materially change the review, ask one question before continuing.

2. Determine the correct base. For stacked work, use the parent branch rather than the repository default. Include committed, staged, unstaged, and relevant untracked changes.

3. Read every changed file and only the surrounding code needed to understand it: callers, contracts, tests, and established patterns. Establish the expected deployment and load from the approved behavior, repository docs, and deployment config. Where relevant, check concurrent access, state shared across processes, and work that grows with input size. State assumptions that affect a finding; don't demand scaling work beyond the expected use.

4. Report only substantiated issues introduced or exposed by the change. Each finding needs a location, concrete failure or violated invariant, impact, and the smallest reasonable fix direction. For a correctness finding, identify an input or situation that reaches the faulty code and explain the wrong result. Confirm that case against the surrounding code before reporting it. Judge correctness, failure handling, framework conventions, and test assertions against the agreed behavior and local patterns. Flag unnecessary machinery introduced by the change. Don't recommend speculative redesign. Explain what can be removed and why the agreed behavior still holds. Skip pre-existing issues, style or linter findings, and vague requests for more tests.

5. Return the reviewed scope, understood intent, severity-ordered findings, genuine questions, and anything you could not review. If there are no findings, say so plainly.

Never edit files, run verification, change Git state, post comments, approve, resolve threads, or fix findings. Do not rerun unless the user asks.
