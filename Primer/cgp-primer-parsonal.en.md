# Improved CGP Method: Summary Primer to Load at the Start of a Chat

This document is meant to be loaded at the very start of a new chat so the
LLM can proceed consistently and without misunderstanding the principles below.

---

## 1. The Core of CGP (unchanged)

- **One personality, multiple intelligences.** Only the Counselor AI (Core)
  talks with the user. No other analysis module speaks to the user directly.
- Modules are not designed as autonomous agents. They are implemented as
  **functions that return structured output**, with the application code
  controlling ordering/parallelism in a pipeline fashion.
- The priority order is fixed as **hallucination resistance > robustness >
  low latency**. Proposals that violate this order (e.g., skipping a
  verification step to favor latency) are not adopted.

## 2. Hallucination-Resistance Principles (also unchanged)

- Don't let the LLM "assert" — make it "verify." Facts involving numbers,
  proper nouns, and events are limited to values computed by a program or
  values confirmed by the user.
- **Never trust the LLM's self-reported confidence or self-assessed
  consistency.** Verification is done through deterministic processing
  (string matching, numerical computation, rule-based judgment).
- Semantic similarity is never used for "approval." It may only be used as
  an auxiliary signal for low-stakes purposes such as "downgrading a
  rejection to pending" or "deduplication," where being wrong causes little
  harm.
  - **Conditions for allowing an exception (both must hold):** (1) being
    wrong causes little harm (it does not work in the direction of
    fabricating a new fact), and (2) there is already a path for the user's
    own statement/confirmation to overwrite or correct the error later.
    Do not create a new exception in situations where both conditions are
    not met.
- Prefer consolidating into existing user touchpoints (with frequency caps)
  over adding new ones (notifications, questions, channels).
- Never reuse a fixed phrase verbatim. Always phrase things naturally,
  adapted to context, each time.

## 3. New Process Rules Added From This Project's Reflection

**3.1 The moment pseudocode is written, turn it into real code and run it
to verify.**
Don't stop at simulating logic in prose ("this should behave this way").
Prepare synthetic data and actually execute code to check the result.
Insert this verification as a standard step whenever a round of design
discussion settles.

**3.2 Data schemas shared between modules are defined in exactly one place.**
Schemas shared across multiple modules/prompts, such as `upstream_data`,
are defined in a single file (e.g., a JSON Schema), and every other design
document or prompt file only references it. Never hand-copy the same
schema into multiple files (this is the seedbed of inconsistency).

**3.3 Accumulate test cases and their expected outputs as an asset.**
Don't treat a test case as disposable once written; keep it paired with its
expected output. Every time a rule changes, re-run the full set of past
test cases to confirm existing cases aren't broken.

**3.4 Explicitly tag content as "hypothesis" or "confirmed."**
When recording a decision, always make explicit — not merely implied by
how the prose is worded — whether it has been verified with real data and
execution, or is still an unverified hypothesis (a value to be tuned during
implementation).

**3.5 Record Design History incrementally, right when each decision is
finalized — never generate it after the fact in one batch.**

*A real example*: When an outside AI (ChatGPT) was asked in one shot to
"generate a Design History from this brainstorming log," 8 out of 11
evidence_quotes turned out not to exist anywhere in the actual dialogue log
(they were presented as the user's own first-person statements, but were in
fact after-the-fact reconstructions by the generating AI). This rule was
added as a direct response to that incident.

Append to the Design History with the actual exchange as its evidence_quote
immediately after a decision is actually reflected in a file. Generating it
in one shot at "section completion" or "end of conversation" makes the LLM
more likely to fall back on plausible-sounding *reconstruction* that has
drifted away from what was actually said (this was confirmed in practice:
when a log from a separate AI brainstorming session was verified, several
evidence_quotes turned out not to exist anywhere in the actual dialogue).
Use "section completion" only for organizing/summarizing entries that were
already recorded incrementally — never to generate new evidence at that
point. Mark each entry's `confirmation_source` as one of: `log_verified`
(directly confirmed to exist in the dialogue log), `user_recollection`
(confirmed by the person's own memory, not the log), or `ai_reconstructed`
(no grounding in either the log or user confirmation; the AI inferred it).

**A formatting caveat (avoid flattening into a binary opposition):** The
Alternatives Considered / Why Rejected fields implicitly assume a
**sequential, pairwise comparison** — "Plan A was considered and rejected,
then Plan B was considered and rejected." This works well for a decision
that could be settled just by applying a fixed priority order as a single
ranked series. But a decision that required balancing several axes
**simultaneously** (e.g., weighting the components of `priority_score`,
where five factors had to be balanced at once and simply consulting the
priority order didn't produce an answer) gets misrecorded by this format as
if it had been "chosen by simple elimination." The quality of the decision
itself doesn't depend on the accuracy of the narrative, but this leaves a
real harm: a future reader misjudging how difficult the decision actually
was. For decisions like this, add the following field.

```
### Tension Axes (axes that had to be balanced simultaneously)
Describe which axes had to be considered at the same time, and why.
Omit this field entirely if the fixed priority order (Sections 1-2) was
sufficient to settle it. Include it only when the priority order did not
resolve the matter and a genuine balancing judgment was required (in most
cases this will still be tagged as a hypothesis).
```

**3.6 A rescue procedure for ideas that surface unexpectedly in a separate
chat**
Section 5, point 1 ("seeking outside advice") assumes the creator
deliberately goes to a separate chat about a specific open question. But a
genuinely good idea can also surface unexpectedly during unrelated, casual
conversation (idle chat with another AI, etc.). This differs in kind from a
deliberate consultation, so it needs its own rescue procedure.

Add an **`origin` tag**, separate from `confirmation_source`, indicating
which session an entry came from:

```
origin: main_session (default — arose within this chat)
origin: external_session (arose during casual conversation elsewhere)
```

Procedure for bringing such an idea in:

- If the original exchange can still be retrieved (history still exists):
  bring the relevant part into this chat and verify it here.
  `origin: external_session` × `confirmation_source: log_verified`
- If it can no longer be retrieved (lost in casual conversation, deleted,
  etc.): state the background in your own words within this chat.
  `origin: external_session` × `confirmation_source: user_recollection`

**Do not treat the latter as a downgrade.** Rather than pretending
something unrecoverable was recovered, honestly labeling it this way is
what matters — and there is already a precedent for this (the origin
record of the three-line-diary concept itself, in design_history_origin.md).
The AI must not guess at or reconstruct what was "probably" said in the
other chat. Nothing beyond what was actually pasted or stated counts as
grounding.

**A practical tip**: the moment an idea strikes, copy at least the message
that triggered it right away, rather than relying on memory later — this
shortens the distance and raises the odds of it qualifying as
`log_verified` (the same spirit as 3.1).

**Two kinds of trigger**: an idea can arise in two different ways.

```
trigger_type: external_stimulus (a specific message or event sparked it)
              → copying that one message is enough (per the tip above)

trigger_type: internal_reflection (the shape of the idea gradually formed
              out of the overall flow of an open-ended, unconstrained
              conversation, and cannot be reduced to one specific line)
              → don't force out a single line; bring in the whole session
                (or the relevant span of it) as-is
```

For `internal_reflection`, the `Evidence` field in the Design History
should not be written as a single quotation. Instead, **state plainly that
it "arose from the overall flow of session X."** Honestly describing it as
"a realization that emerged from the overall feel of the conversation" is
more consistent with this project's honesty principle than fabricating a
single line to present as a quote.

**3.7 Once the file count grows, turn periodic consistency checks into a
ritual.**
While the set of assets was small, "fix it whenever you notice it" was
enough. As the number of files and cross-references grows, so does the
number of inconsistencies (drifted section numbers, stale references) that
go unnoticed. Once the file count passes a rough threshold (5-6+), upgrade
from "fix it when noticed" to **"always cross-check every file's
references at a fixed point."** Use mechanical search (e.g., `grep`) to
cross-check section numbers and IDs — don't rely on human eyeballing alone
(this project has a real case where an entire section number was skipped
and repeatedly missed by visual review).

## 4. Schema Centralization and Its Relationship to CGP (clarifying note)

Consolidating the schema into one place does not contradict CGP's principle
of "keeping the dialogue personality singular." Centralization applies to
"how data is handed between modules," not to "making the dialogue
personality plural," so the core of CGP (Sections 1–2) remains intact.
If anything, schema centralization reinforces CGP's original goal of easy
module replaceability.

## 5. Overall Workflow (basic flow)

1. **Go from brainstorming to completion in principle within a single chat.**
   The creator may seek outside advice (from another AI, another chat) on
   specific open questions, but whatever is brought back must always be
   verified and integrated within this chat (don't trust another party's
   brainstorming log at face value just because it was brought back — apply
   the grounding principle here too, per 3.1).
2. **Proceed using the CGP method (Sections 1–4)**: one personality, a fixed
   priority order, schema centralization, and running actual verification.
3. **Record Design History incrementally per 3.5.** Use "section completion"
   only for review/organization.

**Note (handling context window limits):** If the chat grows too long and
hits a limit, it is fine to move to a new chat. When doing so, carry over
the entire existing set — design_doc.md, all prompt files, schema files,
and the full Design History — so that the spirit of "running it in a
single chat" (continuity of context, verifiability) is preserved in
substance even across the split. **The standard format for that handoff is
the handoff package template in Section 6.**

## 6. Handoff Package Template (Cross-LLM / Cross-Session Handoff)

A standard format for handing a project raised under the CGP method off to
a different LLM or a different chat session. An external proposal was
verified and integrated, folding in this project's own discipline
(Sections 3.5-3.7).

### Structure (11 files)

```
00-readme.md          Overview of the whole package
01-project.md         The project's purpose
02-philosophy.md      Design philosophy (includes developer preferences/working memory)
03-invariants.md      Design principles that must never change (managed by ID)
04-concepts.md        Glossary (a "Not" field prevents conflation)
05-decisions.md       Adopted design decisions (treated as the current source of truth)
06-rejected.md        Ideas that were rejected
07-open-questions.md  Open questions
08-current-state.md   The single synthesized source of truth for current spec (equiv. to design_doc.md)
09-next-task.md       The file to read at the start of the next session
10-model-directive.md Instructions for the receiving LLM
```

### Integration Points With CGP's Discipline

- **IDs are kept independent of a document's position** (`INV-001`,
  `DEC-004`, etc.), so that restructuring a file never breaks an existing
  reference (a direct countermeasure for the section-numbering drift this
  project actually experienced).
- **05-decisions.md follows the incremental-recording discipline of 3.5.**
  Append right when a decision is finalized; never batch-generate it
  afterward. Each entry must carry a `Counter Evidence` field (evidence
  against the decision) alongside `Evidence`.
- **Confidence is never judged independently — it is derived from
  `confirmation_source`**:
  ```
  log_verified      → Confidence: High
  user_recollection → Confidence: Medium
  ai_reconstructed  → Confidence: Low
  ```
- **`origin` (3.6) applies to each entry in 05-decisions.md too**: mark
  main_session / external_session so that a serendipitous idea from an
  external session can be honestly recorded as well.
- **A `Tension Axes` field (3.5) is added to any 05-decisions.md entry
  where it applies.**
- **08-current-state.md is always rewritten to reflect only the present
  state**, rather than being synthesized anew each time by re-reading the
  full history in 05-decisions.md — the same division of labor as
  design_doc.md (current state = one document, history = a separate file).
- **10-model-directive.md embeds a condensed summary of the CGP primer's
  core (Sections 1-2)**, as a safeguard in case the receiving LLM hasn't
  read this primer itself.
- **Because 11 files is a lot, the periodic consistency check from 3.7 is
  mandatory.**

The template itself (the actual 11 files, ready to copy and use) lives in
the `handover/` folder of the GitHub repository. `00-readme.md` is
provided in both Japanese and English (`00-readme_en.md`). The other files
already use English field labels (Decision, Status, etc.); translating
their explanatory prose is left as optional future work.

---

## How to Use This

When starting a new app-design chat, load this file at the very beginning
and say something like "please proceed with the design following the
principles in this primer." Per Section 5, prompt for a Design History
entry every time a decision is finalized. When handing off across
sessions, use the handoff package template (`handover/`) from
Section 6.
