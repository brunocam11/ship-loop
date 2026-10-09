---
name: slice
description: Turn an agreed feature into small, reviewable implementation steps. Use before building when the work needs decomposition or a durable plan; keep one step when the change already fits one review.
---

Plan one behavior the user can review in one sitting. Stop splitting when it fits.

1. Read the relevant code and repository instructions. Identify existing patterns, affected callers, and compatibility risks before proposing changes. For compatibility-sensitive changes, follow affected stored data, external contracts, and lifecycle behavior. Record the key safety assumption and supporting evidence, marking anything unverified. Include a check that could disprove the assumption under Checks and runner, assigned to the agreed runner.
2. Prefer vertical slices with observable behavior. Avoid doing all backend work before all frontend work. A foundation step is reasonable when it is independently useful and verifiable. Record dependencies and agree shared interfaces where needed; don't prescribe extra flags, mocks, or abstractions by default. For new or changed shared interfaces, include one short caller example under Interfaces and identify who owns mutable state where relevant. Use this to agree observable behavior while leaving internal implementation flexible.
3. Use the template below for each step. Goal, Files, Test assertions, Excluded, Finish line, and Checks and runner are required. For a low-impact change that needs no new test, say why under Test assertions. Keep Interfaces only when a shared API, type, or schema changes. Keep the failure table for I/O; include meaningful failures, not every imaginable exception. Omit it otherwise. Add material decisions only when relevant. The finish line is behavior that runs end to end. Final is never a finish line. Later steps may change this code; say what is provisional. If nothing runs after about two hours, the step is too big.
4. Present the plan for approval before implementation. Reuse one plan file per feature. If none exists, agree its path with the user before creating it. Preserve the approved plan and progress there so work can resume after a context reset.

```markdown
# <Feature>

## Step 1: <behavior>
Status: proposed / approved / implemented / reviewed / shipped
Goal: <observable outcome>
Files: <expected files and their responsibilities>
Interfaces: <shared API, type, or schema changes, if any>
Test assertions: <behavior to prove, or why no new test is needed>
Excluded: <work deliberately outside this step>
Finish line: <what makes this step complete>
Checks and runner: <commands or manual checks; agent or user for each>

Failures (I/O only):
| Failure | Handling | Test |
| --- | --- | --- |
| <meaningful failure> | <agreed response> | <assertion or manual check> |
```

Keep internal implementation choices flexible within the approved behavior. If the diff grows beyond about 300 changed lines including tests, explain the growth and consider splitting; this is a review checkpoint, not a hard limit.

Each step uses `next-task`, guided user review, then a fresh AI review. The user predicts behavior before the implementation explanation. Fixes need approval; publishing a reviewed PR needs explicit authorization. Stacking, worktrees, and delegation are optional when the work benefits from them.
