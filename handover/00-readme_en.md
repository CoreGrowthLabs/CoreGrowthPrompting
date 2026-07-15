# 00-readme.md (English)

## Package Version
1.0.0

## Project Name
[FILL IN]

## Last Updated
[FILL IN: YYYY-MM-DD]

## Source LLM
[FILL IN: e.g. Claude Sonnet 5]

## Target LLM
[FILL IN: e.g. a different chat / a different model]

## Overview
[FILL IN: 2-3 lines describing what this project is]

---

## About This Package

This set of files is the standard format for handing a project raised
under the CGP method (Core Growth Prompting) off to a different LLM or a
different chat session. It is defined in Section 6 of the CGP primer
(`CGP方式_改良版プライマー.md` / `cgp-primer-personal_en.md`).

**The receiving LLM must not start work having read only `Current State`
(08).** Always read in the following order.

## Reading Order

1. 00-readme.md (this file)
2. 01-project.md
3. 02-philosophy.md
4. 03-invariants.md
5. 04-concepts.md
6. 05-decisions.md
7. 08-current-state.md
8. 09-next-task.md
9. 06-rejected.md / 07-open-questions.md / 10-model-directive.md (reference as needed)

## Operating Rules (see CGP primer 3.5-3.7)

- Append to `05-decisions.md` right when a decision is finalized. Never
  batch-generate it after the fact.
- Because there are many files, periodically cross-check every file's
  references (IDs, section numbers) mechanically (e.g., with `grep`).
- When bringing in a serendipitous idea from a separate session, always
  mark the `origin` tag (main_session / external_session).

---

*Note: the explanatory prose in files 01 through 10 of this template is
currently in Japanese, while all field labels (Decision, Reason, Status,
etc.) are already in English. This file (00-readme) is provided fully in
English as the primary orientation point for non-Japanese-speaking users;
translating the remaining files is left as optional future work.*
