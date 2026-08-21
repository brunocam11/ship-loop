---
name: continue
description: Pick up work handed off from a previous conversation. Use at the start of a fresh session after /handoff saved the continuation prompt.
---

Resolve the repository root with `git rev-parse --show-toplevel`. If the current directory is not in a Git repository, use the current directory. Read `~/.ship-loop/handoffs/<repo-basename>.md`, where `<repo-basename>` is the basename of that resolved directory.

If the file does not exist or is empty, say there is no pending handoff for this repository and stop.

Treat its contents as the user's instructions for this session. Read every file listed under **Start here** before taking any other task action, then follow **Begin**.

Delete the handoff file after reading it successfully. The handoff is single-use; durable state belongs in the files it references.
