# One Pager

Use case: Scenario B · ICA passport applications. All data in this file is synthetic and labelled as made up.

## Problem and user

The user is a passport applicant in the online application and renewal channel — someone who does this perhaps once every 10 years. The most confused group is parents applying for a child.

The problem: 17% of submissions are returned for errors or missing documents, adding ~6 days each. Applicants don't know what a complete application looks like for their situation. The status quo is a generic guidance page, and only 38% read it before applying — so generic guidance isn't working. 520,000 applications and renewals a year go through this channel.

Constraints: final decisions stay with ICA officers. The vendor withdraws in 10 weeks and the date won't move.

## Opportunity and evidence

The moment: pre-submission — the stage just before an applicant submits. An automated vetter checks the draft application against the checklist for the applicant's situation and tells them exactly what would get it returned, before they submit.

Hypothesis: by providing an automated vetter to passport applicants before they submit, I can reduce the return rate of applications, reducing the amount of time overall. Riskiest assumption (kill switch): applicants will follow instructions from an automated vetter — if they make no change after seeing the suggestions, stop.

Evidence (synthetic user quotes):

- "I renewed mine fine, but for my daughter I had no idea I needed both parents to sign — it came back after a week."
- "I only found out the photo was too old after they returned it. Why couldn't it tell me before I submitted?"
- "The guidance page is long and none of it felt like it was about my case."

Success metrics: returns rate by officers below 5% (from 17% today; success signal for the first experiment is 17% to 10%). Supporting metrics: agreement rate between vetter and officer manual checks, changes by applicants post-vetting, median end-to-end application time.

## Experience and trust
## Risks (market, technical, team)
## Build plan and handoff
## Agent role and guardrails
## Roles and operating model
## Strategy and roadmap

### Strategy kernel

**Diagnosis:** Applicants cannot understand the complexity of the application form. They submit this perhaps once every 10 years or so, and they submit it as best they can, but they have no idea where the pitfalls are.

**Guiding policy:** Take the applicants themselves out of the loop where possible.

**Coherent actions:**

1. Simplify the form steps
2. Change data/information needed so it can be pulled
3. Pulling information on behalf of the applicants where possible

## Metrics plan and stop line
## Evals and gates
## Decision: continue, pivot or stop
## Open issues
