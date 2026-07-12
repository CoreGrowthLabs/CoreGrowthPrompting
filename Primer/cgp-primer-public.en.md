# CGP Method (For High-Publicness Cases): Overview

Redesigned from personal-use CGP (single persona, hallucination resistance
first) for public contexts where values may genuinely conflict across
multiple stakeholders. The premises differ from personal-use CGP, so this
should be treated as a separate method rather than reused as-is.

---

## 1. The fundamental difference from personal-use CGP

Personal-use CGP worked because it could ground everything in a **single
fact**: did the person actually say this? High-publicness issues (privacy
vs. transparency, efficiency vs. inclusion, speed vs. deliberation,
automation vs. human oversight, etc.) have no single fact to ground in to
begin with. So the core of personal-use CGP — "keep a single conversational
persona" — is dropped, and the new core becomes: **"structure the conflict
without erasing it."**

## 2. First triage: can it be grounded, or is it contested?

Before starting work on any point, determine:

```
Can this point be settled by confirming a single fact?
        ↓
Yes → Apply conventional fact-based grounding verification
No  → Tag it "contested" and route it to the Value-Tension Module
```

## 3. Value-Tension Module (conflict detection)

- A dedicated module that checks whether a point matches a known pattern
  of value conflict (privacy vs. transparency, etc.). **It only detects —
  it never resolves.**
- The AI's role is limited to being a "scanner": searching broadly across
  past similar cases and precedents from other fields to surface candidate
  conflicts. The AI never self-determines and finalizes "this is a
  conflict" (an application of "never trust the LLM's self-report").
- The list of known conflict patterns is created and maintained in advance
  by humans — not a single maintainer, but multiple people with different
  attributes and standpoints. AI suggestions are only candidates; they are
  never added to the list automatically.

**Note on future extension (if adding a prioritization feature)**: the
current Value-Tension Module only detects conflicts — it has no mechanism
for ordering which of several simultaneously detected conflicts should go
to deliberation first. If a future version adds a feature analogous to
personal-use CGP's `priority_score` — ranking multiple detected conflicts —
that prioritization logic will itself become a judgment that balances
several axes at once (urgency, scope of impact, disparities in who can
easily raise their voice, etc.), running into the same problem as "Tension
Axes" in personal-use CGP Section 3.5. At that point, recording the
decision only in an Alternatives Considered format risks flattening what
was actually a multi-axis simultaneous balancing act into a misleading
sequential comparison ("rejected A, then rejected B"). When adding a
prioritization feature, bring personal-use CGP's Tension Axes field into
the public version's Design-History-equivalent record as well.

## 4. Asymmetric bias: when in doubt, treat it as contested

Personal-use CGP's Journal Module favored "tolerate over-detection rather
than missed detection." The public version reverses this. **Because the
cost of missing a conflict (a less-heard party continuing to bear harm) is
greater than the cost of over-detecting (more deliberation overhead)**,
when in doubt about whether something is a conflict, treat it as one.

## 5. Redefining what counts as "confirmed"

```
grounding_status (personal use):
  confirmed = grounded in something the person actually said

deliberation_status (public version):
  reviewed = there is a record of engagement from the actually
             affected stakeholder group(s)
  ai_drafted_only = only an AI-drafted item with no record of
                    stakeholder engagement
```

An item tagged `ai_drafted_only` never goes into the "verified" column, no
matter how internally consistent it is.

## 6. Never write the "choosing" logic into code

- Holding multiple options (Option A, Option B) side by side with their
  supporting rationale is technically easy, and should be done actively.
- But **the moment you write code that decides "which one wins in the
  end," that logic itself becomes a new value judgment.** Never write it.
- The actual choice is delegated to a legitimate human process outside the
  code — voting, a deliberative body, consensus-building among the
  stakeholders, etc.
- Even where a conflict-resolution rule exists, it must remain something
  the code merely executes — a governance rule the stakeholders agreed to
  in advance — never something the code invented.

```python
result = {
    "status": "contested",
    "options": [
        {"label": "Privacy-first option", "rationale": "...", "supporting_data": {...}},
        {"label": "Transparency-first option", "rationale": "...", "supporting_data": {...}}
    ],
    "resolution": None  # Intentionally left empty. The code never chooses.
}
```

## 7. Traceability (audit log)

As the public-version counterpart to personal-use CGP's "AI profile
(accumulated relationship)," maintain an **audit log recording which
decision traces back to which stakeholder's statement, and when.** Without
this, the AI's generated plausibility ends up substituting for actual
representativeness.

**The timing discipline is the same as personal-use CGP (Section 3.5)**:
log immediately after a decision is finalized, never reconstruct it in
bulk afterward. Reconstructing the audit log all at once after a
discussion ends creates distance from what was actually said, and risks
the AI producing a plausible-sounding fabrication of events (a problem
actually confirmed during personal-use CGP verification). In the public
version, this fabrication risk can distort the record of a specific
stakeholder's statement — a bigger consequence than in the personal-use
case.

## 8. Publish the post-hoc learning record

When a previously missed conflict comes to light later, record it with the
same logic as personal-use CGP's `rejected_patterns`, building up a
pattern library so the same kind of structure isn't missed again. In the
public version, though, **this record itself is published, so it can also
be verified by external eyes.**

## 9. An external channel for raising objections

Provide a channel through which external stakeholders or groups can flag a
conflict that isn't on the internal list or deliberation structure. This
is a matter of institutional design, not technology — but it's essential
for covering blind spots the Value-Tension Module misses.

## 10. Make it explicit in the deliverable

At the point a draft is produced, **embed a statement — not as a
disclaimer, but built into the format of the deliverable itself — that
"this is an internally consistent AI-generated draft that has not yet been
verified by stakeholders."**

## 11. Guidance on model selection (standard tier vs. frontier tier)

Following the same idea as personal-use CGP (Section 6), the public
version also treats **distinguishing a "standard tier" from a "frontier
tier"** as an operational rule. In the public version, though, Section 6's
principle — "never let the AI make the final choice" — takes precedence,
and tier selection only ever operates inside that constraint.

As in personal-use CGP, **define the tiers relative to what the operating
body can actually access, not as an absolute performance rank.** This
matters especially in the public version, since the operating body — a
local government, an NPO, a multi-organization joint body — may have
different contracted plans and access conditions from one case to the
next. Anchoring "frontier" to "the single most capable model that exists
anywhere" would make feasibility vary wildly by organization and stop
functioning as usable guidance. Define "frontier tier" as the most
capable option that operating body can actually contract for and use, and
"standard tier" as the cost-efficient option that same body uses day to
day — so this section doesn't need rewriting as more models become
restricted to specific plans or contracts in the future.

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

---

## Summary: what technology can and can't handle

| What technology (AI / code) can handle | What only a human structure can handle |
|---|---|
| Grounding fact-based points | Final determination of a conflict |
| Broadly surfacing conflict candidates (scanner) | Maintaining the conflict-pattern list (multiple people, diverse standpoints) |
| Holding multiple options side by side with rationale | Deciding which option actually wins |
| Recording the provenance of a decision (audit log) | Running the channel that receives objections |
| Embedding "AI-drafted" status into the format | The institution that publishes records for external verification |

CGP in high-publicness contexts carries a heavier weight than the
personal-use version toward "designing what the AI must not do." The core
of it is keeping the AI's role limited to a blind-spot-prevention aid — a
broadly cast scanner — rather than a final decision-maker.
