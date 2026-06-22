---
name: slice
description: Turn an approved plan into an ordered stack of small, independently-reviewable PRs, written in place as the feature's progress ledger. Use after a plan is approved and before implementation, when work needs splitting into manageable PRs that survive across conversations.
---

One job: an approved plan into an ordered PR stack you can ship one at a time without blocking review. Stop the moment the plan is one reviewable PR; don't manufacture slices.

1. Size is one of three split axes. ~800-1k LOC incl. tests is a comfortable single PR, but split below that bar too whenever any one of these holds: separate review concerns (e.g. a risky LLM-prompt change vs pure presentation, different eyes and different risk), or a deploy/verify ordering dependency (a later slice can't be verified until an earlier one ships). If none hold and it's one concern under the bar, say so and stop, hand straight to the build loop.
2. Lock the contract (cross-cutting only). The shared types / API shape / schema change is what makes the rest independent. If it moves, it's PR 1, its own small PR, expand/contract so it stays reversible. If the feature spans repos (separate front and back), the contract is slice zero: publish the API spec or shared types first as its own deliverable, then backend and frontend slices proceed in parallel against it, frontend building against a generated mock and types.
3. Each PR is self-contained and ships with its own tests. It can be a full vertical slice (API + UI + test) or a single layer — backend and frontend don't have to land in the same PR — as long as it's independently reviewable and doesn't break the build. Many thin > few thick.
4. Order so the stack is always safe to merge. Every PR keeps the build and the app green on its own (new code paths stay behind a flag or unwired until ready), and the last PR wires it all together so the feature actually works end to end. Then classify each slice: `depends-on` (blockers first), size (S/M/L), AFK (mergeable unattended) vs HITL (needs a human decision/review), and any feature-flag / migration note. Non-code steps (backfill, re-synthesis, config/flag flips) go in the stack as `ops` entries, not PRs.
5. Decide what parallelizes. A dependent chain (PR2 needs PR1) is built as stacked PRs: each slice branches off the previous one's branch, so you start the next as soon as the prior is greplooped, without waiting for the merge. Slices with no dep edge between them are independent: run them in parallel in separate git worktrees, one session each. Stack only the chain.
6. Quiz to approval. Present the numbered stack and ask: granularity right? deps right? anything to split or merge? Iterate until approved.
7. Write the ledger in place. Rewrite the existing plan doc into the template below, same file, don't spawn a new one (only create `.claude/<feature>.md` if no plan doc exists). It stops being temporary; it is now the progress cursor. `gh pr list` / `git log` is the source of truth and the ledger is only the cursor, so always reconcile against git when resuming in a new conversation.

```
# Feature — <name>
Contract: <shared types / API shape / schema>   # omit if not cross-cutting
Cursor: next = PR<n>   (PR1 #<id> merged, ...)

- [ ] PR1 — <end-to-end behavior, not layer-by-layer>   deps: none   ~S   AFK
- [ ] ops — <non-code step: backfill / re-synth / flag>  deps: PR1    after deploy
- [ ] PR2 — <...>                                        deps: PR1    ~M   HITL  (flag: x, migration: expand)
Decisions: <one-liners resolved while slicing>   # distilled to your decision memory on ship (optional)
```

Each slice then runs the build loop (write the test, implement, greploop, ship), ticked and PR-linked on merge. On full ship: distill `Decisions` to your decision memory (optional), delete the ledger; git and the merged PRs remain the record.
