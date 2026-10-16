---
name: review-checklist
description: Reviews a product brief against a fixed ten-point checklist (owner, success measure, scope consistency, problem before fix, open questions, dependencies, evidence, timeline, non-goals, staleness). Use when the user asks to review a brief, run the review checklist, or check a brief before it goes any further, and points at a brief file or pastes one.
---

# Review checklist for briefs

Run the same ten checks, in this order, on whatever brief the user points at (a file path, a wiki page, or pasted text). Read the whole brief first. If no brief is identified, ask which one.

## The checks

1. **Names who owns it.**
   Look for one named person (not a team, not "TBD", not a role with no name). If the brief and any other record (roadmap, wiki page) name different owners, flag that as a fail and say who each names.

2. **Says how we'll know it worked.**
   Look for a measure of success: a metric, a before/after comparison, or an observable outcome. Pass only if it says what is measured, and ideally a baseline and a target or threshold. "Improve the experience" or "users will be happier" is a fail.

3. **Scope at the end matches scope at the start.**
   Compare what the brief says it will do at the top (summary, goals, in-scope) with what it actually lists at the bottom (requirements, phases, next steps, open items). Flag anything that appears at the end but not the start (scope creep), and anything promised at the start but missing at the end. Note anything listed as out of scope that is later reintroduced.

4. **Explains the problem before it proposes a fix.**
   Check that the problem (who is affected, what happens, evidence) appears before the first proposed solution, and that the problem section stands on its own without the fix. A brief that opens with the solution, or whose "problem" is just the absence of the proposed feature, is a fail.

5. **Open questions and risks are named, with an owner.**
   Look for what the brief admits it doesn't know or hasn't checked (untested assumptions, unsized work, unreviewed changes). Pass only if each one is written down and has a named person or a clear next step. An admission in passing with no owner is Partial.

6. **Dependencies and who needs telling.**
   Look for every team or person the work affects or relies on (other products, designers, engineers, support, data). Pass only if each is listed with what is needed from them or what they need to be told. Flag any who are mentioned but not carried into the decisions or next steps.

7. **Claims are backed by evidence, and guesses are labelled.**
   Check that numbers and causes say where they come from, and that anything unproven is called a hypothesis, estimate or guess. Flag figures with no source and starting values presented as settled.

8. **Has a timeline or decision date.**
   Look for a date, a release, a phase order, or a date by which a decision is needed. "Soon" or no mention is a fail.

9. **States what it will not do.**
   Look for explicit non-goals or exclusions. Pass if there is a clear list. Fail if none, since an unbounded brief invites scope creep.

10. **Is current.**
    Check the date, and whether claims about status (shipped, committed, owned, "ready to build") still match other records the user has available, such as release notes, the roadmap or the wiki. Only compare against sources you can actually read; if you can't check, mark Partial and say what was not checked.

## Output format

Start with one line: the brief reviewed and an overall verdict (Ready to go further / Needs changes first).

Then a table:

| Check | Result | Evidence |
| --- | --- | --- |
| 1. Owner named | Pass / Fail / Partial | Quote or point to where in the brief |
| 2. Success measure | Pass / Fail / Partial | ... |
| 3. Scope matches | Pass / Fail / Partial | ... |
| 4. Problem before fix | Pass / Fail / Partial | ... |
| 5. Open questions owned | Pass / Fail / Partial | ... |
| 6. Dependencies | Pass / Fail / Partial | ... |
| 7. Evidence | Pass / Fail / Partial | ... |
| 8. Timeline | Pass / Fail / Partial | ... |
| 9. Non-goals | Pass / Fail / Partial | ... |
| 10. Current | Pass / Fail / Partial | ... |

Then, for each Fail or Partial, one or two sentences on what specifically to change. Quote the brief rather than paraphrasing where it helps.

## Rules

- Check only these ten things. Do not add other criticisms (style, length, feasibility) unless the user asks; if something else serious stands out, mention it in one line after the table, labelled "Outside the checklist".
- Judge only what the brief says, except check 10, which may compare against other records. Do not fill gaps from other sources or assume an owner, measure or date that is not written down.
- Any Fail makes the verdict "Needs changes first". Partials alone do not, but list them.
- Do not edit the brief unless asked. This skill reports; the user decides what changes.
- Keep the language plain and the same every time, so results are comparable across briefs.
