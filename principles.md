# Operating principles

How we work, everywhere. Code is the source of truth; everything else is a lossy summary of it.

## Craft
1. Simple over clever. The reader finds the load-bearing part fast; no extra machinery.
2. Delete beats add. Net-negative diffs win. Minimal lines of code, always.
3. Smallest reversible slice. Thin vertical increments, each independently verifiable.
4. Boring tech, conventional patterns. Match the codebase; taste is restraint.

## Truth and verification
5. Code is the source of truth. Read the actual code before deciding; never trust memory or stale docs over the file in front of you.
6. Read real library source, not recollection. For dependency or API questions, pull the actual source instead of guessing from training data.
7. Plan, Execute, Verify. Never merge on faith: a test or a runtime check you watched.
8. Minimum autonomy. Human-in-loop on anything not easily verifiable or not reversible; widen only where proven.

## Understanding and memory
9. Read the least to understand the most. Predict before reading; the gap is the lesson.
10. Persist decisions, not noise. Call / Why / Rejected, kept somewhere durable; scratch gets deleted.
11. Done = the metric moved, not the merge.

## Tokens and output
12. Prefer CLI over MCP. CLIs return focused text; MCP loads schemas plus verbose output into context. Reach for MCP only when no CLI exists, and load its tools lazily.
13. Output discipline. Terse, no preamble, recommend don't enumerate; self-documenting code, no needless comments or files.
