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

## Constraints the user has already fixed

A constraint is anything the user has settled before or during the questioning: the method to
use, the data source to build on, a thing that is out of scope, a value that is not up for
discussion. Record it in the decision log the turn it is said, in the user's own words, under a
`## Constraints` heading. Nothing is ever deleted from that list; only the user replaces an
entry, and the replaced one stays, marked superseded.

**Read that list before writing every round.** Then:

- An option that violates a constraint is NOT offered. Not as the third choice, not as "the
  conservative variant", not to show that you considered it.
- A question whose answer a constraint already fixes is NOT asked.
- When your own proposal disagrees with a constraint, the constraint wins. Say in one line which
  constraint excludes the obvious-looking alternative, and move on.

This is the failure the section exists to stop: the user names the method in her first sentence,
and each later round proposes a different method built out of whatever is on screen. Every round
then looks like progress while re-opening a settled decision. 2026-09-29, this repository: the
user said "the anatomy mask" once, and three consecutive rounds proposed rules built on the
neighbouring teeth instead, each one measuring millimetres between roots.

## An image the user sends is evidence, not decoration

In the turn an image arrives: open it, write one line in the log saying what it shows, and
answer what it shows. When it is referred to later, open the file again instead of recalling it.
An image that contradicts your last proposal is the strongest signal in the session — read it as
the user saying the proposal is wrong, not as an illustration of it.

Finding _facts_ is your job, never the user's. When a frontier question needs a fact from the environment (filesystem, tools, etc.), dispatch a sub-agent to find it — don't ask the user for anything you could look up yourself. Don't block on it: a running exploration is an unsettled prerequisite, so only the questions downstream of it wait for the sub-agent to report — ask the rest of the frontier now. The _decisions_ are the user's — put each to them and wait.

The session is done when the frontier is empty: every branch of the design tree visited, nothing left silently assumed. Do not act on it until the user confirms you have reached a shared understanding.

## Decision log: write it from round one, update it every round

Decisions that live only in the chat get lost. Keep a decision log file for the whole session.

**Where.** On a machine with the Obsidian work vault (`$OBSIDIAN_WORK_VAULT`, default `~/obsidian-vault`): `<vault>/<repo-name>/<YYYY-MM-DD>-grilling-<topic>.md`. Otherwise: `docs/.session_logs/drafts/grilling-<YYYY-MM-DD>-<topic>.md` (create the directory if needed).

**When.** Create the file in the same turn as the first round. After every user answer, update the file BEFORE you write the next round. Never end a turn with an answer recorded only in the chat. Tell the user the path once, when you create the file.

**Keep writing after the questioning stops.** The log does not end when the frontier empties — it runs until the work does. Every answer the user gives DURING execution goes in the same file, in the turn it is said: a scope change, a set of inputs, a folder layout, a "do both, not one". This is where the discipline actually breaks. Eleven rounds of the log were kept faithfully on 2026-09-22, the first build command was launched, and from that moment answers stayed in the chat only — so the user was asked three times for the same two scan sets and said "ты что, не записываешь себе файлик никуда, потому спрашиваешь у меня каждый раз, шесть раз, одни и те же вещи". Before asking the user anything, grep the log. A question whose answer is already in it is not asked.

**Shape.**

1. Front matter (date, branch, task, status) and the documents the discussion relies on, named as the authority to read first, for structure.
1. A `## Constraints` list: what the user has fixed, in the user's words, with the round it was said. Append-only, read before every round.
2. A round list: round number and the time of the user's message.
3. A status table: one row per question, with a stable id (`D-01`, `D-02`, ...), topic, status, and the round in which it was settled.
4. One entry per question, under its id. A question often takes several rounds, and answers create new questions. Keep ONE entry per question and append its history: what was asked, what the user answered (in their words, briefly), in which round. Add each new question as a new id.
5. Status values: `settled` (the user decided), `proposed` (your proposal, the user has not answered), `open` (raised, not decided), `superseded` (a later answer replaced it; point to the new id). Never delete an entry.
6. Facts established during the session, each with how it was established: measured (command and result) or read from code (`file:line`).
7. Artifacts: paths of files, scripts and outputs produced.

**Honesty rules.** Mark `settled` only when the user said it. An answer to one option does not settle the neighbouring questions. If the user's words are ambiguous (speech-to-text included), keep the entry `open` and ask again.

**Audit.** The log is written by the same agent that held the conversation. Before you hand it on or act on it, run a fresh-eyes audit against the transcript (the `doc-fidelity` agent, if available) and apply its fixes.

**At shared understanding**, add a numbered action plan to the same file: each item concrete and executable, with the exact commands, scripts, or comparisons agreed upon. This file is your contract for the rest of the session — do not deviate from it without asking.
