# CGP Method (For High-Publicness Cases): Overview

A redesign of personal-use CGP (single persona, hallucination-resistance as
top priority) for public-interest contexts where multiple stakeholders may
hold genuinely conflicting values. The premises differ from personal-use CGP,
so this is treated as a separate method rather than a direct reuse.

---

## 1. The Fundamental Difference From Personal-Use CGP

Personal-use CGP worked because it could ground itself in a **single fact**:
"did this person actually say this." In high-publicness issues (privacy vs.
transparency, efficiency vs. inclusion, speed vs. deliberation, automation vs.
human oversight, etc.), there is often no single fact to ground in at all.
So personal-use CGP's core principle — "keep one dialogue persona" — is
dropped, and the new core becomes: **"structure the conflict and preserve it,
rather than resolving it away."**

## 2. First Triage: Groundable, or Contested?

Before starting on any issue, judge the following:

```
Can this issue be settled by confirming a single fact?
        ↓
Yes → Apply conventional fact-based grounding verification
No  → Tag it "contested" and route it to the Value-Tension Module
```

## 3. Value-Tension Module (Conflict Detection)

- A dedicated module that checks whether an issue matches known patterns of
  value conflict (e.g., privacy vs. transparency). **It detects only — it
  never resolves.**
- The AI's role is strictly limited to being a "scanner": searching broadly
  across past similar cases and precedents from other fields to surface
  candidate conflicts. The AI never gets to decide on its own that "this is a
  conflict" and finalize that judgment (this applies the principle of never
  trusting the LLM's self-reported judgment).
- The list of conflict patterns is created and maintained in advance by
  humans — and not by a single maintainer, but by multiple people with
  different attributes and standpoints. AI suggestions are only candidates;
  they are never added to the list automatically.

## 4. Asymmetric Bias: When In Doubt, Treat It As Contested

Personal-use CGP's Journal Module followed a policy of "tolerate misses over
false positives." The public-interest version reverses this. **Because the
cost of a miss (a quieter stakeholder continuing to be disadvantaged) is
greater than the cost of a false positive (more deliberation overhead),**
when in doubt about whether something is contested, it is treated as
contested.

## 5. Redefining What Counts as "Confirmed"

```
grounding_status (personal use):
  confirmed = grounded in something the person themselves said

deliberation_status (public-interest version):
  reviewed = there is a record of involvement from the actually-affected
             stakeholder group(s)
  ai_drafted_only = the AI merely drafted it; there is no record of
                    stakeholder involvement
```

Items tagged `ai_drafted_only` are never placed in the "verified" column, no
matter how internally consistent they are.

## 6. Don't Write the "Choosing" Logic Into Code

- Holding multiple options (Option A, Option B) side by side, each with its
  own rationale, is technically easy — and is actively encouraged.
- However, **the moment you write into code the logic for "which one to
  finally choose," that logic itself becomes a new value judgment.** This is
  never written.
- The actual choice is left to legitimate human procedures that sit outside
  the code — voting, deliberative councils, consensus-building among
  stakeholders, etc.
- Even where conflict-resolution rules do exist, the state to maintain is
  that the code did not invent those rules — it merely executes governance
  rules that the stakeholders themselves agreed to in advance.

```python
result = {
    "status": "contested",
    "options": [
        {"label": "Privacy-first option", "rationale": "...", "supporting_data": {...}},
        {"label": "Transparency-first option", "rationale": "...", "supporting_data": {...}}
    ],
    "resolution": None  # Intentionally left empty. The code does not choose.
}
```

## 7. Traceability (Audit Log)

As the public-interest counterpart to personal-use CGP's "AI profile
(accumulated relationship history)," this version keeps an **audit log
recording which decision traces back to which stakeholder's statement, made
when.** Without this, the AI's generated plausibility ends up substituting
for actual representativeness.

## 8. Publish the Record of Post-Hoc Learning

Cases where a missed conflict later comes to light are recorded using the
same idea as personal-use CGP's rejected_patterns, and accumulated as
patterns so the same kind of structure isn't missed again next time. In the
public-interest version, however, **this record itself is made public, so it
can also be verified by outside eyes.**

## 9. A Channel for External Objections

A channel is provided through which outside stakeholders or organizations —
ones not already on the internal list or deliberation body — can flag
conflicts that were missed. This is a matter of institutional design rather
than technology, but it's essential for compensating for blind spots in the
Value-Tension Module.

## 10. Making This Explicit in the Output

At the moment a draft is produced, **an indication that "this is an
internally consistent AI-generated draft that has not yet been reviewed by
stakeholders" is embedded directly into the format of the deliverable
itself — not tucked away as a disclaimer.**

---

## Summary: Separating What Technology Can and Cannot Solve

| What technology (AI/software) can handle | What only human institutions can handle |
|---|---|
| Grounding fact-based issues | Finally determining what counts as a conflict |
| Broadly surfacing candidate conflict patterns (scanner) | Maintaining the conflict-pattern list (multiple people, diverse standpoints) |
| Holding multiple options side by side with rationale | Actually deciding which option to choose |
| Recording the provenance of decisions (audit log) | Running the channel that receives objections |
| Embedding "this is an AI draft" into the format | The institution that publishes records and enables external verification |

CGP in high-publicness contexts leans much more heavily than the personal
version toward "designing what the AI is *not* allowed to do." The core of
it is continuing to limit the AI's role to an auxiliary one for preventing
oversights — a wide-net scanner — rather than a final decision-maker.
