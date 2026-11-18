# Prototype: passport application vetter (Scenario B)

All data in this prototype is synthetic and labelled as made up on the page itself.

## What this is

A click-through demo of the moment from the AI UX flow: an applicant reviews a draft
passport application and clicks "Check and submit my application" instead of plain Submit.

## What is built so far

**The full three-step flow** (`index.html`):

- Step 1 — simulated Singpass login (prompt chain stage 1, Expand): a pretend login
  button (no real authentication, nothing retrieved) that "pulls" made-up records.
  The service starts generic — "Renew a passport" — and after login (or continuing
  as a guest) the applicant picks what they are renewing: their own passport (not
  built; the prototype says so) or a child's passport, the test case. On the child
  path, a child picker lists the children linked in the records — two made-up
  children with names one letter apart, to test wrong-child selection — plus
  "My child is not in this list" (typed child = a warning asking for proof of
  relationship, never a block). "Continue without Singpass" starts a guest run:
  nothing is prefilled and the vetter checks every field in full. Logging out
  clears everything that was prefilled.
- Prefilled fields are locked with a "🔒 From Singpass (simulated)" badge and an
  "Unlock to edit" link. An edited field permanently loses its badge, is marked
  "Entered by you", and goes back into the vetter's full-check list. The vetter
  never flags an unedited prefilled field; its result splits "Checked and fine"
  into verified-from-Singpass and checked-by-hand.
- Prefill failures (prompt chain stage 2, Behavior): a facilitator box on the login
  screen can force a failure for a run. "No records come back" shows a plain-language
  banner — no error codes — and switches everything to manual entry; the vet result
  then notes the details were entered manually, as for a guest. "Partial failure"
  prefills the parent only. The banner's "Try Singpass again" asks before replacing
  anything typed in the meantime. "Session expires at the check step" keeps the whole
  draft and offers re-login or continuing without Singpass (which removes every
  Singpass badge, so nothing unverified is shown as verified). A prefill failure
  never blocks starting or submitting — it costs convenience only.
- Step 2 — verify and add documents: the applicant checks the details and
  uploads what cannot be pulled (birth certificate, a new photo — simulated pickers).
  Items resolved here disappear from the vet result before it ever shows, and the
  facilitator panel counts them separately. Payment shows the fee only; the payment
  method is chosen at a later payment step, outside this prototype.
- Step 3 — check and submit: the vetting flow below, unchanged in behaviour.

The planted case is still a child renewal — the highest-confusion case from the
opportunity notes — with three problems, matching the skill file's few-shot example:
  1. Birth certificate not attached (fixable at step 2 or step 3).
  2. Only 1 of 2 parental consent signatures, with shared custody declared — a signature
     cannot be pulled, so it is always fixed at step 3 (or skipped).
  3. Child's photo on file is over 12 months old, so a new one is asked for
     (fixable at step 2 or step 3).
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
