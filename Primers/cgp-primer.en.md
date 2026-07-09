# CGP Method (Refined): Summary Primer to Load at the Start of a Chat

This document is meant to be loaded at the very start of a new chat, so the LLM
can execute consistently without misunderstanding.

---

## 1. The Core of CGP (this doesn't change)

- **One persona, multiple intelligences.** Only the Counselor AI (the Core) talks
  with the user directly. Other analysis modules never talk to the user directly.
- Modules are not designed as autonomous agents. They are implemented as
  **functions that return structured output**, in a pipeline where the
  application-side code controls ordering and parallelism.
- Priority is fixed as: **hallucination-resistance > robustness > low latency.**
  Any proposal that violates this order (e.g., skipping a verification step to
  prioritize latency) is not adopted.

## 2. Principles of Hallucination Resistance (this doesn't change either)

- Don't let the LLM "assert" things — make it "verify" them. Factual numbers,
  proper nouns, and events are limited to values computed by the program or
  confirmed by the user.
- **Never trust the LLM's self-reported confidence or self-consistency
  evaluation.** Verification is done through deterministic processing (string
  matching, numeric computation, rule-based judgment).
- Semantic similarity is never used for "approval." It's only used as an
  auxiliary tool for things like "downgrading a rejection" or "deduplication" —
  cases where being wrong causes little real harm.
  - **Conditions for allowing an exception (both must be met):** (1) being wrong
    causes little real harm (it doesn't work in the direction of fabricating new
    facts), and (2) there's already a path for the user to overwrite or correct
    it later through their own statement or confirmation. If both conditions
    aren't met, no new exception is created.
- Rather than adding new touchpoints with the user (notifications, prompts,
  channels), prioritize integrating into and capping the frequency of existing
  touchpoints.
- Don't repeat fixed boilerplate verbatim. Rephrase naturally each time,
  depending on context.

## 3. New Process Rules Added From This Round's Reflection (new)

**3.1 The moment pseudocode is written, turn it into real code and run it to check**
Don't stop at simulating logic in text alone and concluding "this should work."
Prepare mock data, actually run the code, and check the result. Insert this
verification as a standard step every time a round of design discussion reaches
a natural pause.

**3.2 Define shared data schemas between modules in exactly one place**
Schemas shared across multiple modules or prompts — like `upstream_data` —
should be defined in a single file only (e.g., a JSON Schema), with other
design documents and prompt files merely referencing it. Never hand-copy the
same schema into multiple files; that breeds inconsistency.

**3.3 Accumulate test cases and their expected outputs as assets**
Once created, test cases are not thrown away — they're accumulated together
with their expected outputs. Every time a rule changes, re-run all past test
cases to confirm the change hasn't broken anything that used to work.

**3.4 Explicitly tag "hypothesis" vs. "confirmed"**
When stating a decision, always explicitly mark whether it has been verified
against real data and execution, or whether it's still an unverified
hypothesis (a value assumed to be adjusted during implementation). Don't rely
on tone or phrasing alone to convey this — tag it explicitly.

## 4. Relationship Between Schema Centralization and CGP (clarification to avoid misunderstanding)

Centralizing the schema in one place does not contradict CGP's principle of
"keeping a single dialogue persona." What's being centralized is "how data is
passed between modules," not "making the dialogue persona plural." So the core
of CGP (Sections 1 and 2) remains fully intact. If anything, centralizing the
schema strengthens CGP's original aim of making modules easy to swap out.

---

## How to Use

The intended use is: at the start of a new app-design chat, load this file at
the very beginning, and tell the LLM, "Please proceed with the design
following the principles in this primer."
