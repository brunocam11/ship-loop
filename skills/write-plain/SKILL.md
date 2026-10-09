---
name: write-plain
description: Draft or revise prose in a clear, conversational voice while preserving meaning and technical precision. Use for explanations, documentation, PR descriptions, messages, and rewrites of supplied text.
---

# Write plainly

Write like you're explaining something to a friend who wants to understand it. Be direct, thoughtful and concrete. Keep technical terms when they help, and explain unfamiliar ones through what they do.

## Voice and explanation

- State the point early, then give the reader enough context to follow it. Explain the problem before the mechanism. Answer the next likely question close to where it comes up.
- Connect details to consequences. Say what a choice lets someone do, why a failure matters, or what a number means in practice. Use a concrete example when it makes the explanation easier to follow.
- Explain decisions through the reason and the meaningful tradeoff. Distinguish what happened, what you infer and what you would try next. Admit limits plainly, without apologizing or overselling.
- Use everyday words around precise technical terms. Prefer “the admin page” to “the HTML entry point”. Contractions and natural transitions like “so” and “but” are welcome. Don't force slang, personal asides or rhetorical questions to sound human.
- Let sentences vary in length. Keep related ideas together in connected paragraphs. Cut repetition and ceremony, but keep the reasoning the reader needs. Concise does not mean clipped.
- Prefer periods and commas. Avoid semicolons and use dashes sparingly, especially for asides. Keep necessary hyphens in technical names and syntax. Use headings, lists and tables when they make the material easier to follow, without forcing every answer into a template. No emojis.

## Drafting and revising

Draft from the supplied facts and intent. Never invent the author's experiences, motives, evidence or certainty.

When asked to rewrite, improve wording and structure while preserving meaning, useful specifics and the author's recognizable phrasing. Don't polish away personality or add deliberate mistakes. Keep qualifications, numbers and technical identifiers accurate. Flag a factual ambiguity instead of silently resolving it.

When asked only for feedback or proofreading, stay within that scope. Respect any requirement that the author write the text themselves. This skill does not override project authorship rules.

For a rewrite, return the revised text directly unless the user asks for an explanation or an unresolved issue needs their attention.

## Examples

These illustrate choices for particular passages and audiences, not a required structure or wording. Use the actual document's context and facts. A different passage may need a different approach.

Before: “Request processing is delegated to an asynchronous worker to ensure responsiveness.”

After: “The request saves the message and queues a job, then returns. The worker calls the model, so a slow reply doesn't keep the request open.”

Before: “The model achieved zero false approvals with a review fraction of 43.8%.”

After: “The model passed none of the invalid citations. That still leaves 67 of 153 cases for a person to review.”

Before: “An LLM judge was excluded due to the additional validation burden and the sufficiency of deterministic checks for the evaluated behaviors.”

After: “I left out a judge because it is another model with its own error rate, and someone has to check its judgments too. For these cases, the deterministic checks covered what mattered.”

Before: “Your concern about holding database locks during the provider call is valid. The call has been relocated outside the transaction to prevent other requests from being blocked while awaiting the provider response.”

After: “You're right, other requests needing that row would wait on the provider too. I moved the call outside the transaction so we only hold the lock while changing the database.”

These examples assume the underlying behavior and counts are known. Add clarity, not unsupported detail.
