# CGP Method (For High-Publicness Cases): Overview

**Version: 1.2** (references personal CGP v2.3's Sections 3.1-3.9;
Sections 11-12 added with corrected references)

A redesign of the personal-use CGP (one personality, hallucination
resistance prioritized) for high-publicness contexts where value conflicts
can arise among multiple stakeholders. The premises differ fundamentally
from personal-use CGP, so this is treated as a separate method rather than
a direct reuse. This document assumes personal CGP v2.3. If personal CGP
is updated, check the referenced sections (3.5, 3.6, etc.) for consistency.

---

## 1. The Fundamental Difference From Personal-Use CGP

Personal-use CGP worked because it could ground itself in a **single fact**:
"did the person actually say this." High-publicness issues (privacy vs.
transparency, efficiency vs. inclusion, speed vs. deliberation, automation
vs. human oversight, etc.) have no single fact to ground in to begin with.
So personal CGP's core — "keep the dialogue personality singular" — is
dropped, and the new core becomes: **"preserve conflict as structured
information instead of erasing it."**

## 2. The First Triage: Groundable, or Conflict-Laden?

Before starting work on any issue, first make this determination:

```
Can this issue be settled by confirming a single fact?
        ↓
Yes → Apply conventional fact-based grounding verification
No  → Tag as "contested" and route to the Value-Tension Module
```

## 3. Value-Tension Module (Conflict-Detection Module)

- A dedicated module that checks whether an issue matches a known pattern
  of value conflict (e.g., privacy vs. transparency). **Detection only —
  it does not resolve.**
- The AI's role is limited to being a "scanner that searches broadly across
  past precedents and other fields to surface candidate conflicts." The AI
  does not get to decide for itself, "this is a conflict," and finalize
  that (applying the principle of not trusting the LLM's self-assessment).
- The list of conflict patterns is created and maintained by humans — not
  by a single maintainer, but by multiple people of differing backgrounds
  and standpoints. The AI's suggestions are only candidates; they are never
  added to the list as-is.

(For a note on adding a future prioritization feature, see Appendix A.)

## 4. Asymmetric Bias: When in Doubt, Treat It as Conflict-Laden

Personal CGP's Journal Module followed a policy of "tolerate false
positives rather than false negatives." The public version reverses this.
**Because the cost of a false negative (a quieter stakeholder continuing to
be disadvantaged) is larger than the cost of a false positive (more
deliberation overhead)**, when in doubt about whether something is a
conflict, treat it as conflict-laden.

## 5. Changing the Standard for "Confirmed"

```
grounding_status (personal use):
  confirmed = grounded in something the person actually said

deliberation_status (public version):
  reviewed = there is a record that the actually-affected stakeholder
             group was involved
  ai_drafted_only = the AI merely drafted it; there is no record of
                     stakeholder involvement
```

Anything tagged `ai_drafted_only` is never placed in the "verified" column,
no matter how internally consistent it looks.

**A concern: determining "who counts as a stakeholder" can itself be a
value judgment.** The definition of `reviewed` is "there is a record that
the actually-affected stakeholder group was involved," but deciding who
counts as that group can itself be part of the underlying conflict (e.g.,
for some policy, whether "stakeholders" means only direct users, or also
extends to indirectly-affected nearby residents, can change the outcome
entirely). In such a case, run "the scope of who counts as a stakeholder"
itself through the Section 2 triage as a possible conflict first. Don't
let the act of determining who the stakeholders are become an
undetected, one-level-up conflict of its own.

## 6. Never Write the "Choosing" Logic Into Code

- Holding multiple options (Plan A, Plan B) side by side, each with its own
  rationale, is technically easy and should be actively done.
- However, **the moment you write logic into code that decides "which one
  wins in the end," that logic itself becomes a new value judgment.** Don't
  write it.
- The actual choice is left to a legitimate human process outside the code
  (voting, a deliberative body, consensus among stakeholders, etc.).
- Even where a conflict-resolution rule exists, it should never be something
  the code invented — it should remain a governance rule the stakeholders
  agreed on beforehand, which the code merely executes.

```python
result = {
    "status": "contested",
    "options": [
        {"label": "privacy-first plan", "rationale": "...", "supporting_data": {...}},
        {"label": "transparency-first plan", "rationale": "...", "supporting_data": {...}}
    ],
    "resolution": None  # intentionally left empty — the code does not choose
}
```

## 7. Traceability (Audit Log)

As the counterpart to personal CGP's "AI profile (accumulated
relationship)," keep **an audit log recording which decision traces back to
which stakeholder's statement, and when.** Without this, the AI's
plausible-sounding output can end up substituting for actual
representativeness.

**The timing of this record-keeping follows the same discipline as
personal CGP (3.5): record incrementally, right as each decision is
finalized — never generate it in one batch after the fact.** Reconstructing
an audit log after a discussion has ended increases the distance from what
was actually said, creating a risk that the AI writes a plausible-sounding
but fabricated account of "how we got here" (a problem actually confirmed
during verification of personal CGP). In the public version this
fabrication risk is more consequential than in the personal version, since
it can end up distorting the record of what a specific stakeholder actually
said.

## 8. Publish the Record of Post-Hoc Learning

When a conflict that had been missed comes to light later, record it in
the same spirit as personal CGP's `rejected_patterns`, accumulating it as a
pattern to prevent missing the same kind of structure again. In the public
version, however, **this record itself should be published so it can be
verified by outside eyes.**

## 9. An External Channel for Raising Objections

Provide a venue where outside parties and affected groups — not covered by
the internal list or deliberative body — can point out a conflict that was
missed. This is a matter of institutional design rather than technology,
but it is indispensable for compensating for blind spots in the
Value-Tension Module.

## 10. Make It Explicit in the Deliverable

At the point a draft is produced, embed — as part of the deliverable's
format itself, not as a disclaimer tacked on afterward — a statement that
**"this is an internally consistent AI-generated draft that has not gone
through stakeholder review."**

## 11. Guidance on Model Tier Selection (Standard Tier vs. Frontier Tier)

Following the same idea as personal CGP (3.8), the public version also
treats **distinguishing a "standard tier" from a "frontier tier"** as an
operational rule. In the public version, though, Section 6's principle —
"never let the AI make the final choice" — takes precedence, and tier
selection only ever operates inside that constraint.

As in personal CGP, **define the tiers relative to what the operating
body can actually access, not as an absolute performance rank.** This
matters especially in the public version, since the operating body — a
local government, an NPO, a multi-organization joint body — may have
different contracted plans and access conditions from one case to the
next. Anchoring "frontier" to "the single most capable model that exists
anywhere" would make feasibility vary wildly by organization and stop
functioning as usable guidance.

- **Use the standard tier for**: the Value-Tension Module's routine
  scanning work (matching against known conflict patterns, broadly
  surfacing conflict candidates) and organizing the post-hoc learning
  record from Section 8 — high-volume, repetitive scanning and
  organizing work.
- **Use the frontier tier for**: support in working through issues that
  touch the structure of the conflict-pattern list itself, or assistance
  in designing the prioritization logic when multiple conflicts are
  detected at once — **situations where the rework cost of a
  later-discovered blind spot is especially high.** Even here, though,
  frontier-tier AI only goes as far as organizing the issue and presenting
  options; the final call on "which one wins" still goes to the human
  process described in Section 6.
- Record which tier was used in the audit log (Section 7) as well. Tier
  selection is itself an operational decision and should remain
  verifiable after the fact.
- Tier selection never loosens the asymmetric bias in Section 4 (treat
  doubtful cases as contested). Using the frontier tier is not license to
  raise the tolerance for missed detections.

## 12. Post-Resolution Verification

Per Section 6, as long as `resolution` remains `None` (the conflict is
still unresolved), the conflict itself must never become a target of
verification. There is no single fact yet to ground it in, so instructing
the AI to "critically review how this conflict is being handled" at this
stage would make the AI effectively rule on the merits of the conflict —
breaking Section 6's principle.

But **once** `resolution` has been finalized through a legitimate human
process, the situation changes. The finalized decision becomes a fixed,
single fact — in the same sense that "what the person actually said"
functions as a fixed fact in personal-use CGP. Only at this point can the
approach from personal-use CGP Section 3.1 (verify through execution, not
critique) be applied to the post-resolution implementation.

- Restrict the instruction to the AI to: **"Show me, by running it, that
  this implementation correctly reflects the finalized decision."**
  Explicitly forbid asking "was this decision actually correct?" — that
  would reach back and violate Section 6's principle even after the
  decision was finalized.
- Concretely, verify only the following, and only as execution results:
  - Whether the implementation unintentionally executes the rejected
    option's behavior instead of the chosen one.
  - Whether the implementation extends beyond the conditions or scope that
    were assumed when `resolution` was finalized.
  - Whether the actual behavior diverges from the wording of the decision
    recorded in the audit log (Section 7).
- Present any discrepancy found not as verbal commentary but as **a
  reproducible test case**: "feeding this input produced the behavior of
  the option that was supposed to have been rejected."
- Post-Resolution Verification targets only "was the decision correctly
  reflected in the implementation" — never "was the decision itself
  correct." Including the latter would make the AI effectively re-litigate
  the decision, hollowing out Section 6's principle that the AI never
  makes the final choice. If a question about the decision's substance
  arises during implementation, route it back to the human process through
  the objection channel (Section 9) — not as a Verification finding.

---

## Summary: What Technology Can and Cannot Solve

| What technology (AI/programs) can handle | What only a human institution can handle |
|---|---|
| Grounding fact-based issues | Final recognition of a conflict |
| Broadly surfacing candidate conflict patterns (scanner) | Maintaining the conflict-pattern list (multiple people, diverse standpoints) |
| Holding multiple options side by side with rationale | Actually deciding which one to choose |
| Recording the provenance of a decision (audit log) | Operating a channel that receives objections |
| Embedding "AI draft only" into the format | The institution that publishes records and enables outside verification |

CGP in high-publicness contexts carries a heavier share of "designing what
the AI is not allowed to do" than the personal version. The core of it is
keeping the AI's role confined to a supporting one — a broad-net scanner
for preventing oversights — rather than a final decision-maker.

---

## Appendix A: Design Notes for Future Extensions (Not Yet Implemented)

### A.1 Adding a Prioritization Feature to the Value-Tension Module

As it stands, the Value-Tension Module (Section 3) only detects
conflicts; it has no mechanism for ordering which one goes to
deliberation first when several are detected at once. If a future version
adds a feature analogous to personal CGP's `priority_score` — ranking
multiple detected conflicts — that ranking logic itself would become a
judgment that balances several axes simultaneously (urgency, scope of
impact, disparities in whose voice is easier to raise, etc.), running into
the same issue as personal CGP's "Tension Axes" (3.5). At that point,
recording the decision using only the Alternatives Considered format risks
flattening what was actually a simultaneous, multi-axis balancing act into
a false sequential comparison ("Plan A rejected, then Plan B rejected").
When adding a prioritization feature, introduce the Tension Axes field
from personal CGP into the public version's equivalent of Design History
as well.
