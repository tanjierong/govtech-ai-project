# Prototype: passport application vetter (Scenario B)

All data in this prototype is synthetic and labelled as made up on the page itself.

## What this is

A click-through demo of the moment from the AI UX flow: an applicant reviews a draft
passport application and clicks "Check and submit my application" instead of plain Submit.

## What is built so far

**Screens 1 and 2 — the review page and the vet result** (`index.html`):

- Screen 1: a pre-filled child renewal draft. This is the highest-confusion case from
  the opportunity notes (parents applying for a child). Three planted problems, matching
  the skill file's few-shot example:
  1. Birth certificate not attached.
  2. Only 1 of 2 parental consent signatures, with shared custody declared.
  3. Child's photo taken 14 months ago.
- Screen 2: clicking "Check and submit my application" reveals the scripted vet result
  in the skill file's format — Situation, Ready to submit: Not yet, 2 blocking issues
  with fixes, 1 warning, 1 "I am not sure — an officer will confirm" item (the fallback
  behaviour), the checked-and-fine list, and the officers-decide note. The card is
  labelled as scripted, not a live model.
- Interactions (the kill-switch test): every flag is fixable on the spot, inline in the
  result card. "Fix this here" on the birth certificate opens a simulated file picker
  and attaching updates the Documents list above (Not attached becomes Attached). The
  consent fix asks for the second parent's made-up name as a typed signature and flips
  the form's "1 of 2 parents" to "2 of 2". The photo warning has "Replace photo", which
  updates the photo row. No real files are read; every control says it is simulated.
- "Check again and submit" re-counts the blockers — if all are fixed, Ready to submit
  flips to Yes and the application submits (simulated). "Submit anyway, without fixing"
  always works: the check can never block submission, which is the Control trust gap
  from the AI UX flow. The submitted banner records whether the applicant fixed the
  issues or skipped them — the behaviour the kill switch watches.

## What this tests

The kill-switch question from the opportunity notes: will applicants fix what the
vetter flags? This screen sets up the moment; the vet result screen (next step) is
where that behaviour gets observed.

## What is faked

Everything. No form logic, no storage, no model call, no real checklist. The vet
result will be scripted for this one demo case; the real vetting logic lives in the
prompt chain (`day2-validate/PROMPTS.md`, Module 5).

## How to open it

Open `index.html` in any browser. It is one file with no server and no build step.
