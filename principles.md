# Operating principles

How we work, everywhere. Code is the source of truth; everything else is a lossy summary of it.

## Craft
1. Simple over clever. The reader finds the load-bearing part fast; no extra machinery.
2. Delete beats add. Net-negative diffs win. Minimal lines of code, always.
3. Smallest reversible slice. Thin vertical increments, each independently verifiable. Open the PR when it runs, not when it's final.
4. Boring tech, conventional patterns. Match the codebase; taste is restraint.

## Code shape

- Validate external data at its boundary. Keep invariants in the layer that owns them; avoid repeating checks downstream.
- Catch errors only to recover, translate the error, or add useful context. Otherwise let them propagate. Don't disguise failure as success.
- Follow existing patterns. Look first for suitable project code, then platform or standard-library features, then installed dependencies, before writing new code. Choose the first option that fully meets the agreed behavior and fits the project's conventions. Add abstractions, configuration, or fallback behavior only when the agreed behavior needs them.
- Simplification must preserve boundary validation, security, accessibility, and error handling that prevents data loss.

## Truth and verification
5. Code is the source of truth. Read the actual code before deciding; never trust memory or stale docs over the file in front of you.
6. Read real library source, not recollection. For dependency or API questions, pull the actual source instead of guessing from training data. Same for fast-moving product facts — model names/versions, pricing, capabilities. Training cutoff lags reality, especially for AI models. Browse/search to confirm current state before advising, don't assert from memory.
7. Plan, Execute, Verify. Never merge on faith: a test or a runtime check you watched.
8. Minimum autonomy. Human-in-loop on anything not easily verifiable or not reversible; widen only where proven.

## Understanding and memory
9. Read the least to understand the most. Predict before reading; the gap is the lesson.
10. Persist decisions, not noise. Call / Why / Rejected, kept somewhere durable; scratch gets deleted. When the same correction recurs, propose the smallest practical structural fix or automated check, such as clearer ownership, a type, a lint rule, or a regression test. A new check should catch a concrete previous mistake. Keep judgment calls in instructions, and get approval before expanding the task.
11. Done = the metric moved, not the merge.

## Tokens and output
12. Prefer CLI over MCP. CLIs return focused text; MCP loads schemas plus verbose output into context. Reach for MCP only when no CLI exists, and load its tools lazily.
13. Output discipline. Be clear and concise without cutting the reasoning the reader needs. Prefer self-documenting code, with no needless comments or files. For drafting or revising prose, read and follow [write-plain](skills/write-plain/SKILL.md). Apply it directly without requiring the user to invoke it, and reuse the guidance throughout the conversation.
