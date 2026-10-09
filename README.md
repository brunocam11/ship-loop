A small set of skills for building with Claude Code and Codex in steps you can review and understand. Agree the behavior first, give the agent room to implement it, then review before shipping. The workflow is language agnostic.

## The loop

```
research -> approved plan -> implement + delegated checks
         -> guided review -> fresh AI review -> approved fixes
         -> final check -> publish PR
```

Repeat for each small PR. Use `scope`, `grill-me`, and `slice` only when they help define the work. Publishing needs explicit authorization.

## The skills

Use the skills the task needs:

1. `/scope` — turn an ambiguous ask into a defined task: problem statement, what's out of scope, and either a few options or one task with its contract. Skip it when the ask is already clear.
2. `/grill-me` — question unresolved decisions about behavior, dependencies, storage, or structure. Skip it when those decisions are clear.
3. `/slice` — agree small implementation steps in one plan file per feature. Reuse an existing file and ask before creating one. Each step records the goal, files, test assertions, exclusions, finish line, checks and runner, plus failure handling for I/O.
4. `/next-task <plan> [step]` — implement one approved step, run delegated checks, and hand back deviations, evidence, and a reading map. A fresh implementation session is optional.
5. `/review` — compare the complete change with the approved plan and surrounding code. Report concrete findings and unnecessary machinery without editing. By default, Claude reviews Codex's code and Codex reviews Claude's in a fresh context.
6. `/submit` — after review and explicit publication authorization, commit the slice and open a concise PR.

For longer work, `/handoff` saves the current state before a context reset and `/continue` picks it up in a fresh conversation. They store one pending handoff per repository under `~/.ship-loop/handoffs/` and do not change the shipping sequence.

Worktrees, stacked PRs, and delegation are optional for work that benefits from them. Keep one writer and one fresh reviewer for each change.

## Principles and playbook

Keep rules at the level where they apply:

- Shared principles: personal coding rules used by the writer and reviewer. `principles.md` provides portable defaults; use your existing source of truth if you already have one.
- Repository `AGENTS.md`: local architecture, conventions, and commands. Claude's `CLAUDE.md` can import it with `@AGENTS.md`.
- Feature plan: approved behavior, scope, checks, decisions, and progress. Internal implementation details stay flexible.

The skills perform the steps; `playbook.md` describes how to combine them.

## Install

Compare source skills with any installed copies and reconcile local edits first. Install reviewed skills individually in `~/.claude/skills/` for Claude Code or `~/.agents/skills/` for Codex. Don't bulk overwrite an existing collection.

Load your shared principles from `~/.claude/CLAUDE.md` using Claude's import syntax:

```
@/absolute/path/to/ship-loop/principles.md
```

For Codex, put an explicit instruction in `~/.codex/AGENTS.md`:

```text
Before working, read and follow `/absolute/path/to/ship-loop/principles.md`.
```

These examples can point to your existing shared principles file. Invoke the installed skills as needed.

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

- One behavior per review. Around 300 changed lines including tests is a checkpoint to explain growth and consider splitting, not a hard limit.
- Prefer vertical slices. Agree important interfaces and preserve compatibility without adding machinery by default.
- Tests prove agreed behavior. Delegate their execution explicitly; for test-first work, the agent observes a failure and a pass. The user does the final runtime check unless delegated.
- Predict before the implementation explanation, then read together. Approve review fixes before the writer applies them.
- PR descriptions briefly state behavior, exclusions, failure handling, and actual evidence. Keep small commits and resolve or explain dismissed review findings.
- Commits: one line, conventional.
- Use CLIs whenever possible, try to not use MCPs to not bloat out the context window. Much more efficient.

## License

MIT. `grill-me` stays under its author's terms (Matt Pocock).
