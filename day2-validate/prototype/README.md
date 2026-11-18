# Prototype: passport application vetter (Scenario B)

All data in this prototype is synthetic and labelled as made up on the page itself.

## What this is

A click-through demo of the moment from the AI UX flow: an applicant reviews a draft
passport application and clicks "Check and submit my application" instead of plain Submit.

## What is built so far

**Screen 1 only — the review page** (`index.html`):

- A pre-filled child renewal draft. This is the highest-confusion case from the
  opportunity notes (parents applying for a child).
- Three planted problems, matching the skill file's few-shot example:
  1. Birth certificate not attached.
  2. Only 1 of 2 parental consent signatures, with shared custody declared.
  3. Child's photo taken 14 months ago.
- A "Check and submit my application" button. Clicking it shows a placeholder note:
  the vet result screen is the next build step.

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
