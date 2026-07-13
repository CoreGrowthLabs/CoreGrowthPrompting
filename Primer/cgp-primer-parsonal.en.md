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
substance even across the split.

---

## How to Use This

When starting a new app-design chat, load this file at the very beginning
and say something like "please proceed with the design following the
principles in this primer." Per Section 5, prompt for a Design History
entry every time a decision is finalized.
