---
name: pr-description
description: Draft or revise a pull request description from a PR link, branch, diff, or supplied notes. Use when asked for a PR description or PR body draft, including when submit prepares a PR. Returns text for review; does not publish the PR.
---

# PR descriptions

Write the description the way you would explain the change to a teammate sitting next to you. The test Chetan gave: read it out loud. Does it easily make sense? "Details aren't the problem, it's that it's just kind of hard to understand." He also said AI descriptions are "so concise it's kind of hard to read".

The usual failure is shorthand: clipped phrases that only make sense if you have read the code. The opposite failure is over-explaining: walking through every limit and edge case. Aim for a description a reviewer can follow in one read, then open the diff for the details.

## Before writing

Read the diff against the base branch and enough surrounding code to explain it. Use evidence that already exists; don't run tests just to write the description. If something is unclear, say so outside the draft instead of guessing.

## Shape

1. **Ticket line**: `Fixes [RHI-N](link)`, or `Related to`. For a stacked PR, the next line names the parent and the merge order.
2. **How it works today**: a few short spoken sentences on what this part of the product does, for someone who has never opened the code. Then what was wrong or missing, with the real case: the error, answer or output someone saw, quoted or in a code block. Then one sentence on what this PR does about it.
3. **"I made N changes."** One numbered bold heading per change, in words a user or reviewer recognizes, with the main file in parentheses. Under it, a few plain sentences: what used to happen, what happens now, and why that matters. Leave the rest to the diff: size limits, edge cases, field lists, error shapes.
4. **Testing**: short first-person bullets, one result each ("I uploaded two real decks..."). Name the model or environment when the result depends on it. For unit tests, say what they prove in one line, not every case they cover, and say that they fail on the old code when they do.
5. **A closing line**, only when a reviewer must act on something: a deploy step, a migration, a limit they would otherwise misread. One or two sentences, not a section, and not a list of what the PR doesn't do.

A small fix can skip the headings and be a few sentences. Add a section like "Why X and not Y" or "Cost" only when a reviewer will clearly ask.

## Shorthand check

Before returning the draft, find every phrase that only makes sense if you have read the code, and replace it with what the user or the agent sees. From a real draft:

- "the viewer only knew the slide status of attached decks" became "the viewer opens the deck's PDF, but only once it knows the PDF is ready"
- "a deck-only library gets the page-image paragraph" became "the agent's instructions include a paragraph saying it can view pages as images. That paragraph was only added if the user had a PDF or an image"
- "the chat-only list and its labels are gone" became "there were two lists of allowed file types. Now there is one, used by both"

Leave out function names, event names, constants and internal terms ("replay budget", "result shape") entirely; the file in parentheses is enough for a reviewer to find the code. Explain a product name the first time it appears.

## Rules that still apply

- First person singular for the author's work ("I changed"). Never "we" for the author and an assistant.
- Claims as strong as the evidence, no stronger or weaker. Separate what the code enforces from what a prompt asks a model to do.
- What was done, why and how. Leave out the process: precedent PRs, calls, who suggested what, how you found things out.
- No self-undercutting hedges, no checklists of test commands, no emojis, no em-dashes, no praise for the change, no "Generated with" footer or other attribution.

## Example

Approved by Bruno (#1877). Match the voice, not the wording.

> Fixes [RHI-2103](https://linear.app/rhizomeai/issue/RHI-2103)
>
> Stacked on #1876. Merge that one first.
>
> Users can already attach a PowerPoint file to a chat message. When they do, two things happen. The text of each slide is saved so the agent can search it. And in the background, the deck is turned into a PDF. The agent uses that PDF when it needs to actually look at a slide, for example to read a chart. The same PDF is what opens when you click a citation to a slide.
>
> My Documents (/docs) is the other place users upload files. Files there are available in all of their conversations. Until now it refused PowerPoint files:
>
> ```
> Unsupported file type. Allowed: PDF, DOCX, XLSX, CSV, TXT, MD, JSON, JPG, PNG, GIF, WebP
> ```
>
> This PR makes /docs accept them and makes them work the same way as in chat.
>
> I made four changes.
>
> **1. /docs accepts PowerPoint files** (`webapp/src/lib/userDocumentUploadPolicy.ts`)
> There were two lists of allowed file types: one for /docs, and a longer one for chat that added PowerPoint. Now there is one list, used by both.
>
> **2. The PDF is made right after upload** (`webapp/src/pages/api/documents/complete.ts`)
> Chat already started making the PDF as soon as the upload finished. /docs didn't, so the PDF would only be made the first time someone needed it, which adds about a minute of waiting. Now /docs starts it right away too.
>
> [two more changes in the same form]
>
> ## Testing
>
> - I uploaded two real decks to /docs on a dev stack: a 183 MB deck with 5 slides and a 79 MB deck with 50 slides. Both were searchable within seconds, and their PDFs were ready about a minute later.
> - In a new conversation, without attaching anything, I asked about a chart in the 50-slide deck. The agent (Sonnet 5.5) found the deck, looked at slide 31 as an image and read the chart correctly. Clicking its citation opened slide 31.
> - Each of the four changes has a new test, and each test fails on the old code.

## Returning it

Return the exact draft in chat for approval. Drafting does not authorize commits, pushes or edits to a published PR.
