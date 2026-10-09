---
name: submit
description: Commit a reviewed slice and publish its PR after explicit publication authorization. Use after guided review, approved fixes, and the agreed checks; preserve existing reviewed commits.
---

Publish one reviewed slice. Match repository conventions and give the reviewer the context needed to assess it.

1. Confirm publication is explicitly authorized, the user has reviewed the change, blocking findings are resolved, and the agreed checks passed. Use the recorded evidence; don't start new verification without delegation. If a required check or review is incomplete, stop and say what remains.
2. Use an explicitly chosen target or agreed stacked parent as the PR base; otherwise discover the repository's remote default branch. Never assume a branch name. If the target is unclear, resolve it before pushing. If on a shared or default branch, create a task branch such as `feat/short-slug` or `fix/short-slug`.
3. Stage only this slice's changes and leave unrelated work alone. If a commit is needed, use a one-line conventional message such as `feat(auth): add password reset`. No attribution trailers. Preserve existing reviewed commits; don't squash or rewrite them by default.
4. Push the reviewed branch and open its PR against the chosen base. Keep the title and body concise and describe the final behavior. Use this outline, omitting failure handling when the change has no I/O:

   ```text
   Behavior: <what changes and why>
   Excluded: <relevant scope limits>
   Failure handling: <agreed handling of meaningful failures>
   Evidence: <actual check results and any remaining limits>
   ```

   For stacked work, name the parent PR and any required merge order. With `gh`, pass a multiline body using `--body-file`.
5. Return the PR URL and stop. Don't start automatic review or fixes. Merge stays with the user.
