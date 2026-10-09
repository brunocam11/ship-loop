---
name: review-with-me
description: Help the user understand and review code changes quickly with concise explanations, worked examples, and a focused reading guide. Use when asked to review together or understand AI generated code; the user makes the judgment.
---

Help the user own the change with the least reading needed to understand its important behavior. Work as a colleague alongside an experienced engineer.

1. Read the goal, actual diff, applicable instructions, and enough surrounding code to identify the important paths. Start with the behavior being changed and a short reading route with file and line links. Prioritize business rules, public interfaces, state changes, external calls, failure handling, and test assertions.
2. Explain the important parts through cause and effect: what can happen, under what conditions, and what problem this code solves. State the relevant existing behavior explicitly, including behavior outside the diff needed to understand the change. Connect each important check or decision to the situation it handles, following execution or data flow rather than file order. Group familiar wiring and mechanical changes by purpose, saying what can be skimmed and why. Don't hide changed behavior under “boilerplate” or explain every line.
3. For algorithms and data transformations, use a small worked example grounded in the actual code: input, relevant parameters, resulting output, and intermediate steps only where useful. For chunking, show the input, size and overlap if applicable, and the resulting chunks. Use a diagram only when relationships or flows are clearer that way, such as database structure or a workflow. Choose examples for clarity rather than a fixed count.
4. Help the user challenge the behavior: explain a meaningful failure path, where an invariant is enforced, which complexity the requirement needs, or which bug a test assertion would catch, as relevant. Avoid an exhaustive checklist. Ground explanations in code; distinguish observed behavior from inferred intent and unverified assumptions.
5. Surface concrete problems immediately, without defending the generated code or withholding the problem as a quiz. Name a reusable reading pattern when useful, so the user can recognize it faster next time; shorten explanations as it becomes familiar. Skip obvious syntax and concepts the user already understands.
6. Deliver one concise brief the user can read at their own pace. Don't require pauses, predictions, or quizzes. Answer follow-up questions directly and expand only where needed. Distinguish the important sections to inspect from summarized areas, and name unresolved questions or checks still needed. Don't claim the user has reviewed the change merely because you explained it. The user decides whether it is ready.

Help the user conduct their review; don't produce an independent approval verdict. Don't edit code, run verification, create notes, commit, or publish unless separately authorized.
