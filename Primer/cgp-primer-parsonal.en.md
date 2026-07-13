# CGP Method (Improved Version): Summary Primer for the Start of a Chat

This document is meant to be loaded at the very start of a new chat so the
LLM can execute the method consistently, without misunderstanding it.

---

## 1. The core of CGP (this never changes)

- **One persona, multiple intelligences.** Only the Counselor AI (Core)
  talks directly with the user. Other analysis modules never converse with
  the user directly.
- Modules are not designed as autonomous agents. They are implemented as
  **functions that return structured output**, orchestrated by
  application-side code that controls ordering and parallelism (a pipeline).
- The priority order is fixed: **hallucination resistance > robustness >
  low latency.** Never adopt a proposal that violates this order — for
  example, skipping a verification step to save latency.

## 2. Principles of hallucination resistance (this never changes either)

- Don't let the LLM "assert" facts — make it "verify" them. Any
  fact-relevant number, proper noun, or event must be a value the program
  computed or the user confirmed, nothing else.
- **Never trust the LLM's self-report** (its self-assessed confidence or
  consistency). Verification must be deterministic — string matching,
  numeric calculation, rule-based checks.
- Never use semantic similarity as grounds for "approval." It may only be
  used as a secondary aid for low-stakes purposes such as downgrading a
  rejection or deduplicating — cases where being wrong does little harm.
  - **Exception conditions (both must hold)**: (1) being wrong causes only
    minor harm and never fabricates a new fact, and (2) there is already a
    path for the user to overwrite or correct the error later through their
    own statement or confirmation. Do not create new exceptions in any
    situation that fails either condition.
- Prefer integrating into existing touchpoints and capping their frequency
  over adding new ones (new notifications, new prompts, new channels).
- Don't repeat fixed boilerplate verbatim. Rephrase naturally to fit context
  each time.

## 3. New process rules added after this project's retrospective

**3.1 Turn pseudocode into real code the moment you write it, and run it**
Don't stop at simulating logic in prose ("this should work"). Prepare
sample data, actually run the code, and check the result. Insert this
verification step as a standard part of the flow every time a design
discussion reaches a milestone.

**3.2 Define shared data schemas in exactly one place**
A schema shared across multiple modules or prompts (e.g. `upstream_data`)
should be defined in a single file (e.g. a JSON Schema), with every other
design document or prompt file merely referencing it. Never hand-copy the
same schema into multiple files — that's a breeding ground for
inconsistency.

**3.3 Accumulate test cases and expected outputs as an asset**
Don't treat test cases as disposable. Keep them paired with their expected
output. Every time a rule changes, re-run the entire accumulated test suite
to confirm the change didn't break an old rule.

**3.4 Explicitly tag "hypothesis" vs. "confirmed"**
When recording a decision, always make explicit — not just implied by
wording — whether it has been verified against real data and execution, or
is still an unverified hypothesis (a value expected to be adjusted during
implementation).

**3.5 Log the Design History right when a decision is finalized — never
reconstruct it in bulk afterward**
Append the exchange that led to a decision as an `evidence_quote` in the
Design History immediately after that decision is reflected in a file.
Generating this in bulk at the end of a section or conversation makes the
LLM prone to producing a plausible-sounding but inaccurate
reconstruction — distant from what was actually said. (This happened in
practice: when a log of external AI brainstorming was checked, several
`evidence_quote` entries turned out not to exist in the actual dialogue.)
Section-completion time should only be used to review and organize entries
that were already logged incrementally — never to generate new evidence at
that point. Every record must state its `confirmation_source` as one of
three categories: `log_verified` (directly confirmed in the dialogue log),
`user_recollection` (confirmed via the user's own memory), or
`ai_reconstructed` (AI-inferred without grounding).

**Formatting note (to prevent collapsing into a false binary)**: the
Alternatives Considered / Why Rejected format implicitly assumes a
**sequential pairwise comparison** — "considered and rejected A, considered
and rejected B" — which suits decisions that can be settled by applying a
single priority order. But a decision that required **simultaneously**
balancing multiple axes (e.g. weighting five factors at once for a
priority_score, where no priority table alone could settle it) gets
misrecorded by this format as if it were "chosen by simple elimination."
The quality of the decision itself isn't affected by how accurately the
story is told, but future readers are left with a wrong impression of how
hard the call was. So, for decisions like this, add the following field:

```
### Tension Axes (axes that had to be balanced simultaneously)
Describe which axes had to be considered together, and why.
Omit this field entirely if the fixed priority order (Sections 1-2)
was sufficient to resolve the decision. Include it only when the
priority table didn't settle it and a genuine balancing judgment
(often still a hypothesis) was required.
```

**3.6 Verification: make the AI break the design, don't let it critique it**

Once Design History logging for a section is settled, don't ask the AI for
verbal commentary on the design itself ("this part is too complex," "this
looks risky," etc.). That's effectively a form of the **LLM self-report**
already ruled out in Section 2 — switching the AI's role to "reviewer"
doesn't fix a blind spot the model never noticed in the first place, since
the blind spot travels with the model, not the role. Worse, verbal
critique lands unevenly: it means something to a builder who can read the
implications, and sounds like jargon to one who can't — which risks
reinforcing exactly the anxiety CGP is meant to relieve: shipping something
that works without understanding why.

Instead, restrict the instruction to the AI to this form:

> "Construct an actual input or situation that breaks this design, run
> the code, and show me it failing."

- Every finding **must be presented as an execution result.** Only
  reproducible facts — "feeding this test case in produced this actual
  failure (output log)" — count as a Verification finding. Nothing else
  does.
- A test that actually fails is a deterministic fact, in the same sense as
  Section 3.1. The builder doesn't need to read code to see that "the test
  is red" means something is wrong. If a test doesn't fail, the "concern"
  behind it can be dismissed.
- Anything checkable by mechanical cross-referencing against the Design
  History or spec (missing entries, numeric contradictions) should be
  reduced to a deterministic check — string matching, etc. — rather than
  left to the AI's natural-language judgment.
- Anything only the builder can judge — domain-knowledge correctness, such
  as whether an output is actually right in a specialist field — should
  not be asked of the AI as a design-validity question. Show the builder
  the actual output and have **them** judge it. This uses the builder's
  own strength directly, rather than relying on an abstract self-assessment
  like "AI Implementation Readiness."
- Verification is not the final step of validation. Treat it as **a
  screening step that surfaces candidates to be handed off to the
  execution-based verification in Section 3.1.**

This step doesn't conflict with the "one persona" principle (Section 1).
The interface talking to the user doesn't split into multiple personas —
the same interface is simply taking an internal stance of "trying to break
this."

## 4. Schema centralization and CGP (a clarifying note)

Consolidating schemas into one place does not conflict with CGP's principle
of "keeping the conversational persona singular." Centralization applies to
*how data is passed between modules*, not to *making the persona plural* —
so the core of CGP (Sections 1-2) remains fully intact. If anything,
centralizing the schema reinforces CGP's original goal of making modules
easy to swap out.

## 5. Overall workflow (basic flow)

1. **Go from brainstorming to completion in principle within a single
   chat.** The builder may seek advice elsewhere (another AI, another
   chat) on specific questions, but anything brought back must be verified
   and integrated within this chat — never trust another brainstorming
   log's conclusions as-is (Section 3.1's grounding principle applies here
   too).
2. **Proceed using the CGP method (Sections 1-4)**: single persona, fixed
   priority order, centralized schema, and execution-based verification
   throughout.
3. **Log the Design History incrementally, per Section 3.5**, right as each
   decision is finalized. Use section-completion time only for review and
   organization.
4. **Run Verification (Section 3.6) by having the AI actually break the
   design.** Accept only failing execution results as findings, never
   verbal commentary, and follow up any significant finding with the
   execution-based verification in Section 3.1.

**Note (handling context window limits)**: if a chat grows too long and
hits its limit, it's fine to move to a new chat. When doing so, hand off
the entire `design_doc.md`, all prompt files, all schema files, and the
full Design History as-is, so the spirit of "keep it to one chat"
(continuity of context, preserved verifiability) is substantively
maintained.

## 6. Guidance on model selection (standard tier vs. frontier tier)

CGP is a design methodology that doesn't depend on any specific AI tool or
model. In practice, though, **distinguishing between a "standard tier" and
a "frontier tier"** — with cost and accuracy tradeoffs in mind — makes it
easier to sustain CGP's priority order (Section 1) without cutting corners.

**Define the tiers relative to what the builder can actually access, not
as an absolute performance rank.** Anchoring "frontier" to "the single most
capable model that exists anywhere" breaks down as a practical guide once
that model sits behind a paid plan, an invitation-only preview, or some
other access gate the builder simply can't reach. So:

- **Frontier tier** = the most capable and most expensive option the
  builder can actually use under their current plan or API access.
- **Standard tier** = the cost-efficient option that same builder uses day
  to day.

Defining the tiers this way means the guidance doesn't need to be rewritten
as free plans lose access to certain models, or as some models become
available only on specific paid tiers. Whatever counts as the "frontier
tier" simply updates automatically as the builder's own access changes.

- **Use the standard tier for**: repeated verification work such as the
  execution checks in 3.1, re-running the full test suite in 3.3, and
  everyday iterative spec adjustments — anywhere CGP's verification steps
  get repeated many times. Cost efficiency can take priority here.
- **Use the frontier tier for**: decisions that determine whether the
  overall design skeleton is sound, decisions at the core of robustness —
  in short, **"chokepoints" where a mistake means redoing everything
  downstream.** Accuracy takes priority here, and the extra cost is
  justified.
- Record which tier was used for which decision in the Design History
  (Section 3.5). Don't just note "a model was used" — record **which tier
  was assigned to which decision**, so the cost/verification-quality
  tradeoff can be reviewed later.
- The priority order (Section 1: hallucination resistance > robustness >
  low latency) doesn't change based on which tier you use. Using the
  frontier tier is not license to skip the execution checks in 3.1 or the
  full test re-run in 3.3.
- The concrete mapping of "which model counts as standard vs. frontier"
  shifts with the pricing and capability of whatever AI service you're
  using, and with the builder's own access conditions (free plan vs. paid
  plan, etc.). Don't fix this into a permanent table — judge it against
  current pricing, capability, and accessible range each time.

---

## How to use this

At the start of a new app-design chat, load this file first and tell the
model: "Please proceed with the design following the principles in this
primer." Follow the basic flow in Section 5, and prompt for a Design
History entry every time a decision is finalized. Use the guidance in
Section 6 as a rule of thumb for assigning the frontier tier to
chokepoints and the standard tier to everyday iterative verification.
