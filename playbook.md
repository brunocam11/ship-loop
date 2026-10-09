# Playbook

Use only the skills the task needs. Build a new skill only when real work exposes a gap.

## Rules and context

Workflow skills stay language agnostic. Shared principles hold personal coding rules. Repository `AGENTS.md` holds local commands, conventions, and architecture; Claude's `CLAUDE.md` can import it with `@AGENTS.md`.

Keep one approved plan file per feature in the repo, including progress and material decisions. Reuse an existing file; agree the path before creating one. The plan defines behavior and scope, while code and Git show what actually exists.

## The loop

1. Research the relevant code and risks. Use `scope` for an ambiguous ask. Use `grill-me` only for unresolved decisions about behavior, dependencies, storage, or structure; skip implementation trivia.
2. Agree the plan before building. Use `slice` when decomposition helps. Each step needs a Goal, Files, Test assertions, Excluded, Finish line, and Checks and runner. For I/O, include a `Failure | Handling | Test` table. A finish line is behavior that runs, never final code. Note what later steps will likely change. Agree important interfaces, then leave internal implementation choices flexible.
3. Use `next-task` for one approved step. Prefer vertical slices and existing patterns. Stop for approval when behavior, interfaces, dependencies, failure handling, or scope must change. Around 300 changed lines including tests is a checkpoint to explain growth and consider splitting.
4. The agent runs explicitly delegated checks. For test-first work, it observes the intended failure and then the pass. The user runs the app and does the final check unless that is also explicitly delegated. Report actual evidence and anything unrun.
5. Hand back deviations, check results, and a reading map. The user predicts the key behavior before the agent explains the implementation. Walk through the code together until the user understands what will ship.
6. Use the other model in a fresh context as the default AI reviewer: Codex reviews Claude's code and Claude reviews Codex's. Give it the approved plan, diff, and repository instructions. `review` compares the change with the goal and exclusions and reports concrete findings, including unnecessary machinery. It does not fix them.
7. The user approves fixes, the writer applies them, and relevant delegated checks run again. The user completes the final check. Keep one writer and one fresh reviewer.
8. Publish the reviewed PR only with explicit authorization. Publish as soon as a step is reviewed. Holding a reviewed step for code you expect to change is the failure mode. A branch older than half a day means the step was too big. Use small commits that tell the story. The PR description states the behavior, exclusions, failure handling, and evidence briefly. If posting AI findings as PR comments is authorized, record each finding's resolution or reason for dismissal.

## Optional tools

Fresh implementation sessions, worktrees, stacked PRs, and delegation are useful when the work needs them. They are not required for every step. Don't add feature flags, generated mocks, or contract packages unless the agreed behavior or rollout requires them.

`handoff` and `continue` help with context resets. `submit` belongs after review and publication authorization.
