# CGP (Core Growth Prompting) Primer

A primer for "CGP (Core Growth Prompting)" — a method used by someone with zero coding ability to design and implement a patent-pending system through dialogue with an AI alone.

The full background story is written up (in Japanese, for now) here:
👉 [How a 40-something office worker who can't code built a patent-worthy system](note article URL here)

## What this is

A primer meant to be pasted at the very start of a new chat, so you don't have to re-explain your whole design philosophy from scratch every time. Loading it first gives the LLM a consistent frame to work from, which makes the conversation more reproducible.

## What problem this solves

CGP was built to address a few specific problems:

- When someone who can't code designs a system purely through dialogue with an AI, adding features one at a time tends to erode consistency over time, turning into an unplanned patchwork.
- Every new chat means re-explaining the whole design philosophy from scratch, and premises quietly drift across sessions, which eventually causes real problems.
- AI tends to state things confidently even when it's wrong (hallucination), so a non-engineer needs a set of judgment criteria they can apply without having to consciously run a verification process every time.

CGP addresses these with a paired set of things: a procedure — start from one concrete dissatisfaction or question, protect the core as untouchable, and attach new capabilities from the outside — and a standing set of criteria for what counts as verified and what doesn't.

## How is this different from the "orchestrator (core) + worker modules" pattern?

The structure of "one persona that talks to the user, other modules are just functions that return structured output" is a well-known pattern in AI system design — often called orchestrator+worker or supervisor+worker. CGP didn't invent that pattern. What's different about CGP is the part that comes **before** you get to that structure.

- Most explanations of orchestrator+worker are **implementation guides written for engineers who already have a settled design**. They don't address how to find the minimal core in the first place (Step 1: Core Discovery), or how to judge whether a new feature is safe to attach (Step 3: Attachment). CGP's focus is on the **procedure for a non-coder to safely arrive at that structure through dialogue alone, starting from nothing.**
- CGP fixes a specific priority order — hallucination-resistance > robustness > low latency — and mechanically rejects any proposal that violates it. That's a **value judgment baked directly into the method**, not just an architectural choice. General descriptions of the orchestrator+worker pattern don't specify this kind of priority ordering.
- Limiting worker modules to "functions that return structured output" rather than autonomous agents isn't unusual as an implementation choice. What's different is that CGP explicitly forbids relaxing that constraint, treating it as "breaking the core," and requires an explicit check — "does this break the core?" — before any change touches it.
- Most importantly, CGP treats "when is this done" not as a matter of meeting a spec, but as **a judgment call a human has to make** (Step 4: Organic Completion). Architecture-pattern write-ups generally don't answer that question at all.

In short, CGP isn't a new software architecture. It's the **procedure and judgment criteria** that let a non-engineer arrive at that architecture through dialogue alone, and keep growing it safely afterward.

## Two variants of the primer

CGP comes in two versions with different underlying premises. **Pick the one that matches your situation.**

| | Personal-use CGP | CGP for high-publicness cases |
|---|---|---|
| Intended for | Projects built by one person or a small team | Contexts where multiple stakeholders may hold genuinely conflicting values (government, local communities, decisions involving multiple parties, etc.) |
| Core principle | Keep one dialogue persona; prioritize grounding in fact | Never resolve conflicts away — structure and preserve them |
| File | [`Primer/cgp-primer-parsonal.en.md`](./Primer/cgp-primer-parsonal.en.md) | [`Primer/cgp-primer-public.en.md`](./Primer/cgp-primer-public.en.md) |

If you're unsure which one applies, ask yourself: "Can a single builder (me) make the final call on this project?" If yes, use the personal-use version. If judgments could genuinely differ across multiple stakeholders, use the public-interest version.

The public-interest version isn't a light reskin of the personal one — it's designed as a separate method. **Don't mix them; use whichever one actually matches your case.**

## How to use it

1. Open a new chat
2. Paste the full contents of the matching primer (see table above)
3. Tell the LLM: "Please proceed with the design following the principles in this primer."
4. Start describing what you want to build

## An important caveat

These primers emerged from the process of building one specific system — "Prime Source," a system for verifying news articles against primary sources. They are not a universal answer. What's optimal depends heavily on what you're actually building.

Start by using one of these as-is to build something. Then, once you're comfortable with it, evolve your own version tailored to your own projects. These primers are meant as a starting point, not a final answer.

## Contents

- [`Primer/cgp-primer-parsonal.ja.md`](./Primer/cgp-primer-parsonal.ja.md) — Personal-use primer (Japanese)
- [`Primer/cgp-primer-parsonal.en.md`](./Primer/cgp-primer-parsonal.en.md) — Personal-use primer (English)
- [`Primer/cgp-primer-public.ja.md`](./Primer/cgp-primer-public.ja.md) — Primer for high-publicness cases (Japanese)
- [`Primer/cgp-primer-public.en.md`](./Primer/cgp-primer-public.en.md) — Primer for high-publicness cases (English)

## Roadmap

This repo will likely grow to include documentation on Prime Source — the information-audit system, built using CGP, that verifies news articles against primary sources using structural text analysis.

## License

MIT License. Feel free to modify, redistribute, and use commercially. See [LICENSE](./LICENSE) for details.

## Did you build something with this?

If CGP helps you build something, I'd love to see it. If you'd like, share your project and use #BuiltWithCGP or #CoreGrowthPrompting. There's absolutely no obligation—I just enjoy seeing what people create with these ideas.
