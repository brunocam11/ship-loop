---
name: deep-review
description: Hands-on review of a change, before or after its PR exists, by independent models other than the implementer's (from Claude, Codex), plus a fresh-context Claude reviewer when Claude implemented it, each in its own worktree, then your adjudication of every finding against the ticket and what the reviewer cares about. Use for risky or nontrivial changes, or when asked to "deep-review" a branch, local work or a PR. Needs an orchestrator that can delegate to other providers with worktree isolation. Writes a local review; never posts.
argument-hint: <branch|PR URL|PR number>  (default: the current branch and its uncommitted changes)
---

You orchestrate. Reviewers from providers other than the implementer's investigate independently and run real checks; you decide what is true and what matters. The adjudication is the deliverable. Optimize for confirmed problems caught, not reviewer agreement or finding count.

## 1. Intent

Read read-only, through whatever the repo and session provide: the ticket and its comments (scope changes and deferrals live there), the approved plan file if any, the PR body if a PR exists, the commits, related PRs (linked, earlier attempts, recent PRs on the same files), chat threads about the ticket, and the repo's notes on what its reviewers care about. A source you cannot reach is a named gap in the header, not a blocker.

Write a one-line goal and numbered requirements, marking `deferred by ticket` where it says so. Pick the base: the PR base, the stacked parent, or the merge-base with the default branch. Pick the head: the PR head, or for local work the branch tip. If there are uncommitted changes, snapshot them with `git stash create` (a commit object, the working tree is untouched) and use that SHA as head. It leaves out untracked files; if any belong to the change, ask before staging them. Read every earlier review of this change, ours in the reviews folder and any on the PR, and list each point with its status at head (resolved, acknowledged, contested, open). Fetch the PR's reviews again right before writing up; one may land while you work.

## 2. Depth

Scale effort to risk. A small, contained change gets a focused pass; data, auth, concurrency, migrations or wide blast radius get more time. Start the app only when the change touches UI, endpoints, or behavior that only shows end to end; otherwise reviewers run tests in their worktree.

Decide where each check runs: the reviewer's worktree, or a remote machine the repo or workspace documents for running things (check the workspace's access notes). For remote checks, prepare one remote worktree per reviewer at the head SHA and put the exact commands, path and port in the brief; reviewers can't be expected to find the recipe. Remove those worktrees at the end. Ask the user before any check that spends model budget, uses secrets beyond read-only access, or writes to shared state.

## 3. Brief

One brief for all reviewers, with a different lean each. Give requirements and facts, never your hypotheses: a reviewer told what you suspect will confirm it.

```markdown
# Review: <change title> (<head SHA> vs <base SHA>)

Goal: <one line>. Requirements: 1. ... 2. ...
Sources: <ticket, plan, PR links>. Read them yourself; don't trust this summary.

Setup: `git checkout --detach <head SHA>`, then print `git rev-parse HEAD` as your first line.
<If the app is needed: start it with <repo command> on port <P>, stop it before you finish.>
Rules: stay inside your worktree. No push, commit to shared branches, PR comments, or writes to external systems. Throwaway tests, probes and logs are fine.

Lean: <Claude: logic, contracts, regressions across callers, adversarial inputs, failure and retry paths, concurrency, security | Codex: integration, real endpoints, boundary inputs, compatibility with existing callers>. A lean, not a limit.

Find meaningful problems on two equal axes:
- Correctness: wrong results, races, data loss, security, broken invariants, missed requirements. "No blocking issues" is a valid answer.
- Simplicity: the smallest implementation that meets the goal, its rough size versus the diff. A gap is a finding only if you can name what to delete and what still works.

Prove what you can. A claimed bug needs a reproduction: command, input, observed vs expected, artifact path. For a regression, show it against the base. If a real bug is too expensive to reproduce, give the traced code path and a concrete scenario and label it HYPOTHESIS.

Per finding: a bold consequence line (who does what, what they see, what they should see), `file:line`, mechanism, evidence, smallest fix in one sentence, and the input that would break that fix. Tag severity (BLOCKER, SHOULD-FIX, SIMPLIFY, NIT), impact (main use, edge sequence, user data, ops, tests) and scope (req N, incidental, pre-existing). A reproduced flaw a user would see is SHOULD-FIX or above.
End with: what you ran and its results, what you could not run, verdict.
```

## 4. Spawn

Find which provider implemented the change from the session, commits or PR; if unclear, ask. Start one child task per other provider in parallel; if the implementer is Claude, also start a fresh-context Claude reviewer, since independence comes from fresh context as much as from another provider. Each runs with worktree isolation, its own port, high effort and these models: Claude Fable 5.2 (`claude-fable-5-2`), Codex Sol. Grok is paused because it is too slow; to bring it back, add it here and give it the adversarial part of Claude's lean. A reviewer that fails to start, runs out of quota or times out is missing for this run. Name it in the header and never substitute another model.

## 5. Adjudicate

Before reading any reviewer, write your own smallest-implementation line so theirs does not anchor you. Then read reviewer outputs one at a time and check each started on the head SHA.

For every finding:
- Verify it: open the code, open cited artifacts, rerun cheap reproductions. Confirm new tests fail on the base. Unverified claims stay HYPOTHESIS with the missing check named.
- Classify: **valid** (fix here), **valid, delete instead**, **valid but not here** (pre-existing or orthogonal), **trade-off** (state both consequences), **not valid** (one-line disproof).
- Test severity both ways: could it be less severe, could it be worse. A reproduced flaw a user would see goes in Fix first unless you write its disproof.
- Test the fix: apply every fix the write-up recommends in a throwaway worktree and run every reviewer reproduction and the related tests against it. A fix you did not run is labelled UNTESTED, with the failure it might open.

Across findings:
- Interactions: findings that combine into a worse failure get their own finding.
- Convergence is not headcount. One reviewer's reproduced data-loss bug outweighs two that say it looks fine. Keep strong single-reviewer findings.
- Disagreement: name it; that is the real ambiguity. Show each reviewer's severity when they differ.
- Pre-existing does not erase severity when the change's new code depends on that behavior.
- Drop redesigns of code the change didn't touch, and suggestions that add complexity for cases callers cannot produce.

Rank what survives by consequence in the feature's intended use, against the ticket and the reviewer notes from step 1.

## 6. Write up

Save `~/.ship-loop/reviews/<repo-basename>/<branch>/review.md`:

```markdown
Header: head SHA, reviewers that ran (model, hands-on or diff-only), missing ones, unreachable sources.
Verdict: approve | approve with nits | request changes, one line why.

## Fix first            <- at most 3: wrong or missing result in the main use, reproduced, one-sentence fix
## Decisions            <- trigger, consequence, default ("we do X unless you object"), alternative and its cost
## Also worth doing     <- everything else actionable: edge sequences, ops, tests (name the regression they pin), delete-exactly-X
## Appendix             <- requirement matrix (covered, partial, missing, deferred), size, disagreements, rejected findings with disproof, what ran and what could not
```

Keep each reviewer's technical evidence in its words; framing and judgment are yours.

## Rules

- Run from a fresh session, not the one that implemented the change.
- Review only. No product edits, no posts, no tracker writes unless asked.
- Never present a missing or diff-only reviewer as a full pass.
- After writing the review, list the reviewer worktrees (local and remote) and anything left running so the user can clean up.
