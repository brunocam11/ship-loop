---
name: handoff
description: Save a prompt that continues the current work in a fresh conversation with zero context loss, picked up there via /continue. Use when context is getting big, when the user says "handoff", asks for a prompt to continue, or before a natural break in long-running work.
---

Write one self-contained prompt for an agent that knows nothing about this conversation. Save it to `~/.ship-loop/handoffs/<repo-basename>.md`. Create the directory if needed and overwrite any previous handoff for this repository.

Before writing:

1. If the work has a ledger or plan file, update it first so the handoff points at accurate durable state.
2. Inspect the current branch, working tree, and recent commits so committed and uncommitted work are clearly distinguished.

Write for zero shared context. Point to durable files instead of duplicating them. Inline only decisions, constraints, and gotchas that live nowhere else. Name exact paths, branches, commits, and commands.

Use these sections when they contain relevant information:

1. **Mission and constraints** — the goal and what the next agent must not do, including push, PR, verification, or scope boundaries.
2. **Start here** — files to read first, in order, and why.
3. **Done** — completed work and decisions that should not be reopened, with the reason when it matters.
4. **Left** — ordered next actions. Mark deliberately deferred work and why it was deferred.
5. **Gotchas** — environment details, failed approaches, exact gate commands, and intentional behavior that may look wrong.
6. **Begin** — the first concrete action and where to stop for the user.

Calibrate the length to the remaining work. A nearly finished task needs a short handoff; a mid-refactor needs the full structure.

Reply with only the saved path and: `Open a new conversation in this repository and run /continue`.
