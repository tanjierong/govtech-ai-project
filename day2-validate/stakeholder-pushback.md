# Stakeholder pushback

**Scenario:** B · ICA passport applications

A role-played review of the prototype with three synthetic leaders. All roles, quotes and figures are made up. A summary of this lives in `one-pager.md` under Risks.

## 1 · Director who runs the frontline work (passport operations)

**Strongest objection:** a "Ready to submit: Yes" that an officer still returns becomes a complaint on the operations desk, and the "Submit anyway" path creates a record of knowingly accepted defects — applicants will argue "but the checker said it was fine". Checklist ownership is unresolved: who writes it, who updates it when rules change, who owns the "Rules current as of" date?

**Evidence that would change their mind:**

- Agreement rate between vetter and officer manual checks, measured on a batch of synthetic then held-out applications, at or above a threshold the director sets.
- The miss rate (post-vet returns on "Ready: Yes") tracked and published to operations, with a stop line — the director holds the kill switch.
- Checklist ownership on paper: the vetter reads the operations team's published, versioned checklist and never has rules of its own.
- Return-reason data showing the top return reasons are the ones the vetter catches.

**My response:** we'll ensure the vetter and officers agree >95% or 99% of the time, or a level the director is comfortable with. A confidence threshold so the model flags issues only when confident. The checklist comes from the operations team so it is 100% accurate, and we'll establish a process to update the LLM.

**Director's follow-ups (open items):**

1. "A process" needs a number: the longest an applicant could see a stale rule after a change, and the mechanism behind it.
2. The confidence threshold must not swallow the unsure path — "I am not sure — an officer will confirm" must stay visible on the result.
3. 95% vs 99% is thousands of applications a year; the director sets the bar after seeing eval data.

## 2 · Head of product (owns the online application channel)

**Strongest objection:** the demo is one scripted case with planted defects — a screenplay, not a product. The kill-switch signal (applicants make no change after seeing suggestions) has zero behavioural evidence either way; only 38% read the current guidance, and "Submit anyway" could reveal the vetter is a speed bump that lengthens the journey without moving the returns rate.

**Evidence that would change their mind:**

- Fix-through rate from a small usability test (5–10 people) on the prototype: who fixes versus who submits anyway.
- A funnel comparison in time-to-passport terms: today's journey against the vetted journey.
- Proof the one-case demo generalizes across the six situations in the skill file.
- A 10-week scope cut showing the narrowest valuable slice ships inside the vendor window.

**My response:** we'll rapidly user test to check (1) the fix-through rate, to understand if people will actually fix issues, and (2) the overall return rates for the funnel, to understand the potential upside first.

**Head of product's follow-ups (open items):**

1. Name the fix-through pass bar before the test runs, so the test can fail.
2. Return rates can't be measured in a lab: run the paper funnel instead — sample returned applications through the vetter, count what share of real return reasons it would have flagged. That is the upside ceiling.

## 3 · Chief executive sponsor (accountable for outcomes and budget)

**Strongest objection:** the vetter contradicts the strategy kernel. The diagnosis says applicants can't handle the form's complexity and the guiding policy is to take them out of the loop — yet the vetter keeps them in the loop and hands them a to-do list. Why spend the 10 remaining vendor-weeks on the patch instead of the cure (simplify the form, pull data on the applicant's behalf)? Plus headline risk: a government AI that appears to "approve" an application that is then rejected.

**Evidence that would change their mind:**

- The vetter positioned as step one of the strategy: it ships inside the vendor window, and its flag frequency generates the prioritised backlog for the data-pulling integrations. The patch funds and informs the cure.
- The arithmetic: ~88,000 returned applications a year at ~6 days each, against a thin layer on existing infrastructure.
- The headline defused by design: the vetter never says "approved", refuses outcome predictions, and its miss rate is published with a stop line.
- The kill switch as fiscal discipline: a bounded 10-week experiment with a pre-named failure condition, not a programme commitment.

**My response:** (not yet given — open.)

## What this changes in the plan

- The user test (fix-through rate, with the bar named in advance) and the paper funnel come before any further build.
- The eval data for the agreement threshold is Day 4 work (eval rubric and gates).
- The rule-change lag number and mechanism need an owner and a figure.
- The strategy story for the sponsor: vetter now, data-pulling backlog generated from vetter flags next.
