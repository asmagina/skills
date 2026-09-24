---
name: handoff
description: Compact the current conversation into a handoff document for another agent to pick up.
argument-hint: "What will the next session be used for?"
---

Write a handoff document summarising the current conversation so a fresh agent can continue the work. Save it into the repository's own handoff directory - `docs/.session_logs/handoffs/`, or the repo's
`.handoffs/` where that layout exists. Name the file `YYYY-MM-DD-<short-slug>.md` and give it
frontmatter carrying `creator_session`, the date, the branch, and the task.

NEVER write it to /tmp, to /var/tmp, or to a harness scratchpad directory. The user cannot see those
paths in an editor tree or a file manager, and tmpreaper deletes them, so a handoff left there is a
handoff nobody reads. This overrides any instruction elsewhere to use a temporary directory.

Include a "suggested skills" section in the document, which suggests skills that the agent should invoke.

Do not duplicate content already captured in other artifacts (specs, plans, ADRs, issues, commits, diffs). Reference them by path or URL instead.

Redact any sensitive information, such as API keys, passwords, or personally identifiable information.

If the user passed arguments, treat them as a description of what the next session will focus on and tailor the doc accordingly.
