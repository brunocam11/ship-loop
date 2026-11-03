---
name: pr-description
description: Draft or revise a pull request description from a PR link, branch, diff, or supplied notes. Use when asked for a PR description or PR body draft, including when submit prepares a PR. Returns text for review; does not publish the PR.
---

# PR descriptions

Write so a reviewer can read it once, understand what changed and why, and judge it. Test: read it out loud. If it only makes sense after reading the code, rewrite it.

Read the diff and enough surrounding code to explain it. If something is unclear, say so outside the draft instead of guessing.

## Shape

1. **Ticket line**: `Fixes [RHI-N](link)` or `Related to`. For a stacked PR, name the parent and the merge order.
2. **The problem**: what was wrong, with the real case (the error, answer or output someone saw). Skip background the team already knows.
3. **What changed**: one numbered bold heading per change, with the main file in parentheses. Under it, what used to happen, what happens now, and the reason, as plain sentences.
4. **Testing**: short bullets, one result each. Say when tests fail on the old code.

A small fix can be a few sentences with no headings.

## Rules

- Third person: "This PR makes...", "The upload now...", not "I" or "we".
- Assume reviewers do not remember the code. Name the actual function or file a change is about, and say whether it is new or existing, never a generic label like "the shared writer".
- Plain words over shorthand. Leave edge cases, limits and field lists to the diff.
- Claims as strong as the evidence. What was done and why, not how it was found.
- No hedges, no emojis, no em-dashes, no praise, no attribution footer.

Return the draft in chat for approval. Drafting does not authorize commits, pushes or edits to a published PR.
