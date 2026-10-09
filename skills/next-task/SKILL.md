---
name: next-task
description: Implement one approved step from a feature plan and hand it back for guided review. Use after plan approval, including when resuming in a new session; run only checks delegated to the agent.
---

Args: `<plan>` and optional `[step]`. Reuse the feature's existing plan; if its location or approval is unclear, resolve that before editing. A fresh session is optional.

1. Read the plan, applicable instructions, and relevant code. Compare progress with actual code and Git state; flag drift. Select the requested approved step, or the next approved step whose dependencies are available. If a branch is needed, use the repository's actual base or an agreed parent branch, without disturbing existing work.
2. Confirm the goal, expected files, exclusions, finish line, test assertions, and who runs each check. For I/O, confirm the agreed failure handling. If a material decision is missing, resolve it before building.
3. Implement only this step, following existing patterns and shared coding principles. Choose internal details freely within the plan. Stop for approval if the work needs changes to behavior, interfaces, dependencies, failure handling, or scope. Don't add speculative recovery paths. If the diff grows beyond about 300 changed lines including tests, explain why and consider splitting.
4. Write meaningful tests for the agreed assertions. When test execution is delegated, use test-first where practical: run the test, observe the intended failure, implement, then observe it pass. Don't weaken assertions to get green. Run only the checks delegated to the agent; report unrun or blocked checks plainly.
5. Hand back deviations, actual check results, and a short reading map in that order. Ask the user to predict the key behavior before explaining the implementation. Then walk through the relevant files together, including tradeoffs and any gap between prediction and behavior.

Stop for user review. Don't commit, publish, merge, restack, or advance to another step without authorization. A fresh AI review reports findings; the user approves fixes before they are applied. Update the existing plan as progress is confirmed.
