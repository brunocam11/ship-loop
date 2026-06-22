---
name: scope
description: Turn an ambiguous or undefined ask into a defined task — investigate, clarify only what changes the answer, and output a crisp problem statement plus candidate approaches or one defined task. Use at the start of work when it is not yet clear what the task even is.
---

Turn an ambiguous ask into something you can grill. Stop the moment it is defined enough — not a step more.

1. Restate the ask in one line and name exactly what is unclear.
2. Investigate before asking. Read the actual code, data, and context to ground it — most ambiguity dissolves by looking, not by interrogating the user. Then ask only the few questions that genuinely change the answer: what problem, for whom, what does success look like, what is hard-constrained.
3. Separate the real underlying need from the stated request.
4. Output:
   - a one-paragraph problem statement,
   - what is explicitly out of scope,
   - and either 1-3 candidate approaches to weigh, or one defined task (hand to `grill`).

For anything cross-cutting, the output must include the contract — the API shape, schema change, and shared types — because the contract is what makes the work splittable and parallelizable downstream.
