---
name: scope
description: Turn an ambiguous or undefined ask into a defined task — investigate, clarify only what changes the answer, and output a crisp problem statement plus candidate approaches or one defined task. Use at the start of work when it is not yet clear what the task even is.
---

Turn an ambiguous ask into a defined task. Stop when the problem and scope are clear enough to plan.

1. Restate the ask in one line and name exactly what is unclear.
2. Investigate before asking. Read the actual code, data, and context to ground it — most ambiguity dissolves by looking, not by interrogating the user. Then ask only the few questions that genuinely change the answer: what problem, for whom, what does success look like, what is hard-constrained.
3. Separate the real underlying need from the stated request.
4. Output:
   - a one-paragraph problem statement,
   - what is explicitly out of scope,
   - and either 1-3 candidate approaches to weigh, or one defined task.

Use `prioritize` if available when candidate approaches need weighing. Use `grill-me` only for unresolved decisions about behavior, dependencies, storage, or structure. Otherwise proceed to planning without another interview.

For cross-cutting work, identify the shared API, schema, or type changes that matter to the task. Don't invent new contracts just to satisfy the process.
