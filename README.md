A small set of Claude Code skills that turn a feature into a stack of small, reviewable PRs, shipped one at a time. Pieced together from other people's setups and trimmed down to what actually gets used. Sharing it in case it's useful. Ofc this let you work in multiple tasks/features in parallel.

## The loop

```
scope ─▶ grill-me ─▶ slice ─▶ next-task ─▶ review ─▶ submit ─▶ merge PR
```

`scope` and `grill-me` run once per feature; `slice` onward repeats for each PR until the stack is merged.

## The skills

In the order they run:

1. `/scope` — turn an ambiguous ask into a defined task: problem statement, what's out of scope, and either a few options or one task with its contract. Skip it when the ask is already clear.
2. `/grill-me` — lock the design. It interviews me one question at a time, with its own recommendation each time, until no decision is left open.
3. `/slice` — cut the approved plan into an ordered stack of small, self contained PRs, written to a ledger file in the repo so it survives across sessions.
4. `/next-task <ledger>` — in a fresh session in the repo (better context management), build the next slice off the ledger: reconcile against git, implement using TDD, stop before merge.
5. `/review` — review the complete change against its intent and surrounding code. It reports concrete findings and never edits anything.
6. `/submit` — commit the reviewed slice (one-line conventional commit, no attribution) and open a tight PR.

For longer work, `/handoff` saves the current state before a context reset and `/continue` picks it up in a fresh conversation. They store one pending handoff per repository under `~/.ship-loop/handoffs/` and do not change the shipping sequence.

Independent slices I run at the same time, each in its own git worktree and terminal. Dependent ones stack on each other's branches so I'm not waiting on a merge to start the next. I use Warp for the panes and treehouse for the worktrees.

## Principles and playbook

The skills are the verbs. Two files sit under them and decide how those verbs behave.

- `principles.md` — how I want the agent to work in every session: simple over clever, minimal diffs, read the real code before deciding, verify before merging, CLIs over MCPs. It loads globally (the import in Install below), so it applies no matter which skill is running. Without it the skills still run, but the agent's defaults drift.
- `playbook.md` — how the verbs compose: which skills to chain for a feature vs a bugfix vs a refactor, and the rules for combining them (contract-first, parallel only where it's safe). It's the map for why the loop is shaped the way it is.

Short version: skills do the work, principles and playbook decide how and in what order.

## Install

Copy the skills into your Claude Code skills directory:

```sh
cp -r skills/* ~/.claude/skills/        # global: available in every repo
# or per project:
cp -r skills/* <your-repo>/.claude/skills/
```

Load the principles globally by importing `principles.md` from your `~/.claude/CLAUDE.md`:

```
@/absolute/path/to/ship-loop/principles.md
```

Then invoke a skill with `/scope`, `/grill-me`, `/slice`, `/next-task`, `/review`, `/submit`, `/handoff`, or `/continue`.

## Tools I lean on

- [Claude Code](https://www.anthropic.com/claude-code) — what it all runs in.
- [Warp](https://www.warp.dev) — the terminal I use for the parallel panes ([plugin](https://github.com/warpdotdev/claude-code-warp)).
- [treehouse](https://github.com/mark-hingston/treehouse-worktree) — git worktree manager, keeps parallel slices isolated.
- [claude-mem](https://github.com/thedotmack/claude-mem) — optional memory so decisions carry across sessions. very useful, highly recommended.
- [opensrc](https://github.com/vercel-labs/opensrc) — read a dependency's real source code instead of guessing. Much more effective!
- [caveman](https://github.com/JuliusBrussee/caveman) — compresses output to cut token usage. I run it in ultra. It supposedly cuts around 50% of the tokens usage.
- [no-mistakes](https://github.com/kunchenguid/no-mistakes) — an optional pre-PR gate that runs review, tests, and lint before opening the PR. I haven't used it much yet.

`grill-me` is by [Matt Pocock](https://github.com/mattpocock/skills).

## Conventions

- Self-contained PRs: each one ships with its own tests and stays green on its own. Many thin PRs beat a few thick ones.
- Order so nothing breaks: new code paths stay behind a flag or unwired until the last PR wires the feature together.
- Contract-first for cross-cutting work: lock the API shape, schema, and shared types first. That's what makes the rest parallelizable.
- TDD: failing test, then implement. You own the tests by reviewing, not writing them.
- Commits: one line, conventional.
- Use CLIs whenever possible, try to not use MCPs to not bloat out the context window. Much more efficient.

## License

MIT. `grill-me` stays under its author's terms (Matt Pocock).
