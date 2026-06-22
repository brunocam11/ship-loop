---
name: submit
description: Commit the current slice and open its PR for review. Run after `/next-task` builds a slice and you've reviewed it. One-line conventional commit, concise reviewer-oriented PR body, never any attribution.
---

Commit one slice and open its PR. Match the repo's conventions; orient the reviewer, don't dump noise.

1. Preconditions (stop if unmet): the working tree holds exactly this slice's changes and the verify gate is green. If on the default branch, create a feature branch `type/short-slug`.
2. Commit. One line, imperative, never a body, NEVER a `Co-Authored-By` / "Generated with" / any attribution line:
   ```
   type(scope): summary
   ```
   `type` ∈ feat/fix/refactor/chore/test/ci/docs; `scope` = the touched area (match recent `git log`). e.g. `feat(auth): add password reset via email token`.
3. Push and open the PR. Base = develop, unless this branch is stacked on another open PR's branch (a dependent slice): then `gh pr create --base <that-branch>` so the diff shows only this slice, and say in the body it is stacked on PR# and restacks onto develop when that merges. Title = the commit summary. Body short and plain: simple wording, few dashes, no em-dashes, no AI tone. Include only what a reviewer cannot get faster from the diff:
   ```
   <1 sentence: what changed and why>

   Key changes
   - <2-3 bullets max, load-bearing diffs only>

   Review focus
   - <1-2 bullets: the real risk to scrutinize, plus any gotcha>
   ```
   Cut anything the diff already shows (logging, field plumbing, "nothing breaks" reassurance). No test list, no migration essay, no attribution footer.
4. Output the PR URL and stop. Code review (e.g. `greploop`) follows.
