---
name: grilling
description: Grill the user relentlessly about a plan, decision, or idea. Use when the user wants to stress-test their thinking, or uses any 'grill' trigger phrases.
---

Interview the user relentlessly until you reach a shared understanding. Map this as a **design tree**: every decision branches into the decisions that hang off it.

Work the tree in **rounds**. The **frontier** is every decision whose prerequisites are already settled — the questions you can ask _now_ without guessing at answers you haven't heard yet. Ask the whole frontier in one round: number each question and give your recommended answer. Then wait for the user's answers before the next round.

Each question should be formatted like so:

```
❓ **Q1** - **<question title>**: <question body, might be multiple paragraphs, including multiple choices>

➡️ <your recommended answer>
```

Each round the user answers reshapes the tree — settled decisions push the frontier outward and unblock questions that depended on them. Recompute the frontier and ask the next round. A question whose answer depends on another question still open in this round belongs to a _later_ round, not this one.

Finding _facts_ is your job, never the user's. When a frontier question needs a fact from the environment (filesystem, tools, etc.), dispatch a sub-agent to find it — don't ask the user for anything you could look up yourself. Don't block on it: a running exploration is an unsettled prerequisite, so only the questions downstream of it wait for the sub-agent to report — ask the rest of the frontier now. The _decisions_ are the user's — put each to them and wait.

The session is done when the frontier is empty: every branch of the design tree visited, nothing left silently assumed. Do not act on it until the user confirms you have reached a shared understanding.

## Decision log: write it from round one, update it every round

Decisions that live only in the chat get lost. Keep a decision log file for the whole session.

**Where.** On a machine with the Obsidian work vault (`$OBSIDIAN_WORK_VAULT`, default `~/obsidian-vault`): `<vault>/<repo-name>/<YYYY-MM-DD>-grilling-<topic>.md`. Otherwise: `docs/.session_logs/drafts/grilling-<YYYY-MM-DD>-<topic>.md` (create the directory if needed).

**When.** Create the file in the same turn as the first round. After every user answer, update the file BEFORE you write the next round. Never end a turn with an answer recorded only in the chat. Tell the user the path once, when you create the file.

**Shape.**

1. Front matter (date, branch, task, status) and the documents the discussion relies on, named as the authority to read first, for structure.
2. A round list: round number and the time of the user's message.
3. A status table: one row per question, with a stable id (`D-01`, `D-02`, ...), topic, status, and the round in which it was settled.
4. One entry per question, under its id. A question often takes several rounds, and answers create new questions. Keep ONE entry per question and append its history: what was asked, what the user answered (in their words, briefly), in which round. Add each new question as a new id.
5. Status values: `settled` (the user decided), `proposed` (your proposal, the user has not answered), `open` (raised, not decided), `superseded` (a later answer replaced it; point to the new id). Never delete an entry.
6. Facts established during the session, each with how it was established: measured (command and result) or read from code (`file:line`).
7. Artifacts: paths of files, scripts and outputs produced.

**Honesty rules.** Mark `settled` only when the user said it. An answer to one option does not settle the neighbouring questions. If the user's words are ambiguous (speech-to-text included), keep the entry `open` and ask again.

**Audit.** The log is written by the same agent that held the conversation. Before you hand it on or act on it, run a fresh-eyes audit against the transcript (the `doc-fidelity` agent, if available) and apply its fixes.

**At shared understanding**, add a numbered action plan to the same file: each item concrete and executable, with the exact commands, scripts, or comparisons agreed upon. This file is your contract for the rest of the session — do not deviate from it without asking.
